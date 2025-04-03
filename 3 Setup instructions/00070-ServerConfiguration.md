
### Server configuration

#### Default configuration

The preconfigured Jetty uses these defaults. These can be changed by modifying `{tomcat.base}/lib/oskari-ext.properties`.

Redis:
- running on localhost
- default port (6379)

Database (Postgres with PostGIS extension):
- db URL: localhost in default port (5432)
- db name: oskaridb
- db user: oskari/oskari

Oskari (provided in [download example](/download)):
- url: http://localhost:8080/

#### Custom configurations

1\) Removing the unnecessary parts

The download example uses prebuilt frontend application in the `sample-application` folder and prebuilt server application as `oskari-map.war`. These have repositories that you can use as templates to create your own copies of them:
- https://github.com/oskariorg/sample-server-extension
- https://github.com/oskariorg/sample-application

For modifying what the application initializes for the database content and changing functionalities that you want to include on your application, you will need to create your own modifications and these templates provides an easy way of getting started. One of the easiest way of modifying your application is adding/removing code to/from the application repository, include more dependencies or drop ones that you don't need (like statistical-data related ones if you don't want to use that Oskari functionality in your application).

Similarly, you will need to add/remove references to the functionalities on the frontend application. This is usually done on the `sample-application/applications/geoportal/main.js` file by importing new functionalities or removing unused ones. The frontend application desides what code is included for the frontend. However, it is the database that actually tracks what functionalities are shown on the user interface. Functionalities are packaged as "bundles" in Oskari terms. To use a bundle, the frontend must include it for the frontend build AND the server/database must reference the bundle so it is started by the frontend. You can have bundles that are started based on user roles so not all bundles that are included by the frontend will be shown for all users (for example admin functionalities).

The different kinds of views that the user can see are saved as "appsetup" on the database. An appsetup has some metadata like which JSP-file is used for the appsetup, which frontend application (geoportal/embedded/custom one) is shown and it can have a UI theme to select colors etc for that specific views. A usual setup is having the default appsetup that is shown for all users like a geoportal. Users can use the publisher functionality in Oskari to create new appsetups as they publish maps or even just saving a customized geoportal appsetup.

Appsetups have a list of "bundles" that are linked to an appsetup. This list determines which functionalities are used by that appsetup.
Related database tables are:
- oskari_appsetup
- oskari_appsetup_bundles

2\) Editing article content

- User guide: edit the file in {jetty.base}/resources/articlesByTag/userguide.html
- Publisher terms of use: edit the file in {jetty.base}/resources/articlesByTag/termsofuse__mappublication__en.html

3\) Changing the default port**

- provide port in command line:

    java -jar ${jetty.home}/start.jar jetty.http.port=8080

- change `{jetty.base}/resources/oskari-ext.properties` where ever `8080` is referenced

4\) Proxy settings

If you need a proxy to access internet you can configure it in `{jetty.base}/start.d/oskari.ini`

	-Dhttp.proxyHost=
	-Dhttp.proxyPort=
	-Dhttp.nonProxyHosts=
	-Dhttps.proxyHost=
	-Dhttps.proxyPort=
	-Dhttps.nonProxyHosts=

5\) Database url/name/user/pass are changed
`{jetty.base}/resources/oskari-ext.properties` needs to be updated

	db.url=jdbc:postgresql://[host]:[port]/[dbname]
	db.username=[user]
	db.password=[passwd]

5\) Using external Redis
`{jetty.base}/resources/oskari-ext.properties` needs to be updated

	redis.hostname=localhost
	redis.port=6379
	redis.pool.size=10

6\) How the Jetty bundle was built

See the Howto.md inside the zip-file for details