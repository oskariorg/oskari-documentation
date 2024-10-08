
### Advanced Jetty configuration

#### Jetty default configuration

The preconfigured Jetty uses these defaults. These can be changed by modifying `{jetty.base}/resources/oskari-ext.properties`.

Redis:
- redis running on localhost at default port (6379)

Database (Postgres with postgis extension)
- db URL: localhost in default port (5432)
- db name: oskaridb
- db user: oskari/oskari

Oskari (provided in Jetty bundle)
- url: http://localhost:8080/

#### Custom configurations

1\) Removing the unnecessary parts

Oskari-server can run with just the oskari-map webapp. If you don't need all the features, you can remove them from under `{jetty.base}/webapps`.

You will also need to remove the corresponding parts of the UI so users don't have access to them. This is done by removing "bundles" from "appsetups" (these are Oskari concepts: bundles provide  functionalities and appsetup defines which bundles are used in your app) and currently it needs to be done by modifying the database content. Bundles are linked to appsetups in the database table `portti_view_bundle_seq` and functionalities are removed from the UI by deleting rows from the table.

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