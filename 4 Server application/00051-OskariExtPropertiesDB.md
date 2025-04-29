### Configuring database connections

The usual database configuration can be done with these lines for configuring the URL/database and credentials
```properties
db.url = jdbc:postgresql://localhost:5432/oskaridb
db.username = oskari
db.password = oskari
```
You can also select the JDBC driver class with:
```properties
db.jndi.driverClassName = org.postgresql.Driver
```

#### JNDI context

You can pass database connection through the application servers JNDI-context. Check your application servers manual to see how to define a JNDI resource on that specific server. The default JNDI name is `jdbc/OskariPool` but it can be configured like this:

```properties
db.jndi.name = jdbc/OskariPool
```

#### Flyway modules

You can define different database connections for each Flyway migration module. To enable a module you need to define it in a comma-separated list in:

```properties
db.additional.modules = mymodule
```

After that you can override any database properties for that module using the module name as part of the key like this:

```properties
db.mymodule.url = jdbc:postgresql://localhost:5432/oskaridb
db.mymodule.username = oskari
db.mymodule.password = oskari
```
If you don't override a value for module, one from the non-module-prefixed property key is used (`db.url` is used if `db.[module].url` is not defined etc).

There are a couple of additional flags that can help if you are struggling with migrations:
```properties
# Use autorepair to fix an issue with SQL-script checksums.
# Checksums fail if an updated war-file includes different line-endings than the original one.
# Note! This disable the guard against user-modified scripts so use with caution.
#db.flyway.autorepair=true

# Uncomment to allow application start even if there are migration problems
#db.ignoreMigrationFailures = true

# Validate migration file naming
# false value will skip migrations with invalid naming
# When set to true, flyway will fail the migration, and log the invalid filenames (like having lowercase v as the first character of filename)
#db.flyway.validateMigrationNaming = true
```