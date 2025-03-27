# Setup instructions

This section contains instructions for setting up a new Oskari instance. First a posgreSQL database with PostGIS extension is set up, then the actual Oskari instance with related programs is downloaded and installed. 

This manual focuses on Oskari and gives you the basic guidance to the related software/libraries which should be enough to get you through the setup process. If you encounter issues with PostgreSQL or other Oskari-related libraries, please refer to their respective manuals.

The diagram below shows the system architechture of Oskari.


```mermaid
C4Deployment

Person(user, "User", "")

Boundary(service, "Oskari-based service", ""){
    Boundary(frontend, "Web server", "nginx/httpd"){
        Component(sample-app, "sample-application", "Javascript")
    }
    Boundary(server, "Servlet container", "tomcat/jetty"){
            Component(sample-server, "sample-server-extension", "Java")

    }
    Boundary(db, "Databases", ""){
        ContainerDb(postgres, "PostgreSQL", "PostGIS/SQL", "Application config and user data")
        ContainerDb(redis, "Redis", "", "Caching")
    }
}
System_Ext(sdi, "OGC and other APIs", "External services")


Rel(user, sample-app, "", "")
Rel(sample-app, sample-server, "", "")
Rel(sample-server, postgres, "", "")
Rel(sample-server, redis, "", "")
Rel(sample-server, sdi, "", "")
Rel(user, sdi, "", "")
```