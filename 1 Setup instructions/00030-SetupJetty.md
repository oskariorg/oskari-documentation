## Setup Jetty

This section contains instruction on setting up an instance from a downloaded jetty package with pre-installed/configured Oskari

**For setting up an oskari instance from source code skip this section and move to** <p><a data-internal-anchor="Setup Oskari development environment" href="Setup development environment.md">Setup development environment</a></p>

After this you will have Oskari running including

- Oskari-frontend based sample-application (https://github.com/oskariorg/sample-application)
- Oskari-server based sample webapp (https://github.com/oskariorg/sample-server-extension)
- Geoserver with WPS-plugin and Oskari extensions (https://github.com/oskariorg/oskari-server/tree/master/geoserver-ext)

#### Requirements

The following are required for setting up Jetty.

- JDK 8
- Database available: <a data-internal-anchor="Setup database" href="00020-SetupDatabase.md">Instructions for setting up database</a>
- Redis (Optional, required for WFS and statistical functionalities): [Setup Redis](/documentation/backend/setup-redis)

#### Setting up Jetty

Follow the steps below to get Jetty properly set up.

1\) Download the [Jetty Bundle](/download)

2\) Unpack the zip file to selected location

The zip includes Howto.md, jetty-distribution-9.4.12.v20180830 (referred as `{jetty.home}`) and oskari-server folder (referred as `{jetty.base}`)

3\) Configure the database properties (host/credentials) by editing `{jetty.base}/resources/oskari-ext.properties`

    db.url=jdbc:postgresql://[host]:[port]/[dbname]
    db.username=[user]
    db.password=[passwd]

4\) Startup the Jetty by running (in `{jetty.base}`)

    java -jar ../jetty-distribution-9.4.12.v20180830/start.jar

Note that for folder references it's important where you run the command/what is the working directory so run the command in oskari-server folder and refer to start.jar under the {jetty.home}:

5\) After Jetty is up and running open a browser with URL

    http://localhost:8080


You can login as:
- user with username "user" and password "user"
- admin with username "admin" and password "oskari"

#### Advanced configuration

For further configuration check out <a data-internal-anchor="Advanced Jetty configuration" href="00071-AdvancedJettyConfiguration.md">Advanced jetty configuration</a>
