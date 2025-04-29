### Using database in server code

The database library [MyBatis](https://mybatis.org/mybatis-3/) is used in `oskari-server` for usual database operations and [FlywayDB](https://flywaydb.org/) for migrations.

The `service-mybatis` module offers helpers for handling 
```xml
<dependency>
    <groupId>org.oskari</groupId>
	<artifactId>service-mybatis</artifactId>
</dependency>
```

By convention a service interface/abstract class is made and a concrete `...Impl` class that extends/implements the server service API.

#### Service class

```java
import fi.nls.oskari.service.OskariComponent;

public abstract class MyService extends OskariComponent {
    public abstract MyStuff find(int id);
}
```
##### An example value object for the service

```java
public class MyStuff  {
    private int id;
    private String name;
    
    public int getId() {
        return id;
    }
    
    public void setId(int id) {
        this.id = id;
    }
    
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

#### Implementation class for service

```java
import fi.nls.oskari.annotation.Oskari;
import fi.nls.oskari.db.DatasourceHelper;
import org.apache.ibatis.session.Configuration;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import javax.sql.DataSource;

@Oskari
public class MyServiceMybatisImpl extends MyService {

    public MyServiceMybatisImpl() {
        final DatasourceHelper helper = DatasourceHelper.getInstance();
        // getDataSource takes an optional String parameter as Flyway module id for selecting the connection
        final DataSource dataSource = helper.getDataSource();
        if (dataSource != null) {
            factory = initializeMyBatis(dataSource);
        }
        else {
            LOG.error("Couldn't get datasource for MyService");
        }
    }

    private SqlSessionFactory initializeMyBatis(final DataSource dataSource) {
        final Configuration configuration = MyBatisHelper.getConfig(dataSource);
        MyBatisHelper.addAliases(configuration, MyStuff.class, MyOtherStuff.class);
        MyBatisHelper.addMappers(configuration, MyMapper.class);

        return new SqlSessionFactoryBuilder().build(configuration);
    }

    public MyStuff find(int id) {
        try (final SqlSession session = factory.openSession()) {
            final MyPlaceMapper mapper = session.getMapper(MyMapper.class);
            layer = mapper.find(id);
            return cache(layer);
        } catch (Exception e) {
            LOG.warn(e, "Exception when trying to load category with id:", id);
        }
        return null;
    }
}
```

#### MyBatis mapper

```java
package my.stuff;

import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Options;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;
import org.apache.ibatis.annotations.Update;

public interface MyMapper {
    @Select("SELECT id, name FROM my_table")
    List<MyStuff> findAll();

    @Select("SELECT id, name FROM my_table WHERE id = #{id}")
    MyStuff findById(int id);

    @Update("UPDATE my_table SET name = #{name} WHERE id = #{id}")
    int updateName(@Param("name") String name, @Param("id") long id);

    @Insert("INSERT INTO my_table (name) VALUES (#{name})")
    @Options(useGeneratedKeys=true, keyColumn="id", keyProperty="id")
    void addStuff(MyStuff stuff);

    @Delete("DELETE FROM my_table WHERE id = #{id}")
    void deleteMyStuff(int id);
}
```

Note that the insert operations injects the id of the stored database row into the id-variable of the parameter object.

The SQL can be included in the mapper class annotations like above (recommended) OR in an XML-file in a path that matches the `mapper` class name and package (`src/main/resources/my/stuff/MyMapper.xml`). The id of the SQL operation tags then match the method names of the mapper class:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="my.stuff.MyMapper">

    <delete id="deleteMyStuff" parameterType="Integer">
        DELETE FROM my_table WHERE id = #{id}
    </delete>
</mapper>
```
On complex SQLs these can also be mixed with some methods being annotated and more complex SQLs in an XML file.
