## Setup application server

This section contains instructions on setting up a Java application server for Oskari using the [download](/download) package with Tomcat and pre-installed/configured Oskari

After completing this section you will have the Oskari-based example application running, including:

- Oskari-frontend based sample-application (https://github.com/oskariorg/sample-application)
- Oskari-server based sample webapp (https://github.com/oskariorg/sample-server-extension)

#### Requirements

The following are required for setting up Jetty.

- JDK 17 (Java Development Kit)
- Database available: [Instructions for setting up database](00020-SetupDatabase.md)

#### Setting up Tomcat

Follow the steps below to get Tomcat properly set up.

1\) Download the [Oskari example application](/download)

2\) Unpack the zip file to selected location

The zip includes:

* Readme.md
* licence folder
* oskari-map.war as prebuilt server application
* sample-application folder with the prebuilt frontend application code
* apache-tomcat-10.1.39 (referred as `{tomcat.home}`)
* oskari-server folder (referred as `{tomcat.base}`) 

3\) Configure the database properties (host/credentials) by editing `{tomcat.base}/lib/oskari-ext.properties`

```properties
db.url = jdbc:postgresql://localhost:5432/oskaridb
db.username = oskari
db.password = oskari
```

This assumes you have a PostgreSQL database running on `localhost` (WSL users might need to use `127.0.0.1` instead) on the default port (`5432`) and you have setup a database named `oskaridb` and a user that can access it with credentials `oskari/oskari`. For more configuration options [see oskari-ext.properties](../8%20Configuration%20instructions/00051-OskariExtPropertiesDB.md).

4\) Startup the Tomcat

By running the command (in `{tomcat.base}`):
- `server.bat start` for Windows OR
- `server.sh start` for *nix-based OS (Ubuntu, MacOS, Windows WSL etc)

Note that it's important where you run the command/what is the working directory so run the command in the `oskari-server` folder. Otherwise folder references in those scripts might not work as expected.
The scripts are for convenience only, you could download Tomcat from their [site](https://tomcat.apache.org/) and use that to run Oskari. Separating `{tomcat.base}` and `{tomcat.home}` is useful if you want to use Tomcat binaries as shared library
 (the `{tomcat.home}` folder) and have multiple different Oskari-based applications with their own config and code on their own Tomcat instances. You can add parallel instances by just making copies of the `{tomcat.base}` and changing their configurations.

 The main configurations you might want to take a look at under `{tomcat.base}` when setting up a different setup are:
 - eveything under `lib/*` with the most important file being `lib/oskari-ext.properties`. This is the main configuration file and is searched from the Java classpath on server startup.
 - context XML-files under `conf/Catalina/localhost/*.xml`
 - log files are generated under the `logs` folder

 The zip has `sample-application` and `oskari-map.war` as the actual applications that are served from Tomcat. These are referenced in the `conf/Catalina/localhost/*.xml` files. If you want to have the files somewhere else, you can modify the XML-files to use some other location.

5\) After Tomcat is up and running open a browser with URL

    http://localhost:8080

You can login as:
- user with username `user` and password `user`
- admin with username `admin` and password `oskari`

Note! These default users are introduced in the database migration module for the example application (sample-server-extension) and publicly advertized on the sample-info bundle in the sample-application (frontend repository). You should change/remove these for any applications you create based on the example setup.

#### Advanced configuration

For further configuration check out [Server configuration](../8%20Configuration%20instructions/00040-ServerConfiguration.md)
