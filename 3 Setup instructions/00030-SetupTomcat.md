## Setup application server

This section contains instructions on setting up a Java application server for Oskari using the [download](/download) package with Tomcat and pre-installed/configured Oskari

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

1\) Download the [Oskari example](/download)

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

4\) Startup the Tomcat

By running the command (in `{tomcat.base}`):
- `server.bat start` for Windows OR
- `server.sh start` for *nix-based OS (Ubuntu, MacOS, Windows WSL etc)

Note that for folder references it's important where you run the command/what is the working directory so run the command in the `oskari-server` folder.
The scripts are for convenience only, you could download Tomcat from their site and use that to run Oskari. Separating `{tomcat.base}` and `{tomcat.home}` is useful
 if you want to use Tomcat binaries as shared library (the `{tomcat.home}` folder) and have multiple different Oskari-based applications with their own config and code on their own Tomcat instances.
 You can add parallel instances by just making copies of the `{tomcat.base}` and changing their configurations.

 The main configurations you might want to take a look at under `{tomcat.base}` when setting up a different setup are:
 - eveything under `lib/*` with the most important file being `lib/oskari-ext.properties`. This is the main configuration file and is searched from the Java classpath on server startup.
 - context XML-files under `conf/Catalina/localhost/*.xml`
 - log files are generated under the `logs` folder

 The zip has `sample-application` and `oskari-map.war` as the actual applications that are served from Tomcat. These are referenced in the `conf/Catalina/localhost/*.xml` files. If you want to have the files somewhere else, you can modify the XML-files to use some other location.

5\) After Tomcat is up and running open a browser with URL

    http://localhost:8080


You can login as:
- user with username "user" and password "user"
- admin with username "admin" and password "oskari"

#### Advanced configuration

For further configuration check out [Advanced jetty configuration](00070-AdvancedJettyConfiguration.md)
