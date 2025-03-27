## Setup Tomcat

This section contains instructions on setting up an instance from a downloaded Tomcat package with pre-installed/configured Oskari

**For setting up an Oskari instance from source code skip this section and move to** [Setup development environment](00040-SetupDevelopmentEnvironment.md)

After completing this section you will have Oskari running, including

- Oskari-frontend based sample-application (https://github.com/oskariorg/sample-application)
- Oskari-server based sample webapp (https://github.com/oskariorg/sample-server-extension)

#### Requirements

The following are required for setting up Jetty.

- JDK 17 (Java Development Kit)
- Database available: [Instructions for setting up database](00020-SetupDatabase.md)

#### Setting up Tomcat

Follow the steps below to get Tomcat properly set up.

1\) Download the [Tomcat Bundle](/download)

2\) Unpack the zip file to selected location

The zip includes 
* Readme.md
* oskari-map.war
* licence folder
* sample-application folder
* apache-tomcat-10.1.39 (referred as `{tomcat.home}`)
* oskari-server folder (referred as `{tomcat.base}`) 

3\) Configure the database properties (host/credentials) by editing `{tomcat.base}/lib/oskari-ext.properties`

    db.url=jdbc:postgresql://[host]:[port]/[dbname]
    db.username=[user]
    db.password=[passwd]

4\) Startup the Jetty by running (in `{tomcat.base}`)

    java -jar ../jetty-distribution-9.4.12.v20180830/start.jar

Note that for folder references it's important where you run the command/what is the working directory so run the command in oskari-server folder and refer to start.jar under the {jetty.home}:

5\) After Jetty is up and running open a browser with URL

    http://localhost:8080


You can login as:
- user with username "user" and password "user"
- admin with username "admin" and password "oskari"

#### Advanced configuration

For further configuration check out [Advanced jetty configuration](00070-AdvancedJettyConfiguration.md)
