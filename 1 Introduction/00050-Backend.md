## Backend

Backend functionality of the platform is implemented with Controllers from Spring Framework and can be easily extended to handle new functionality. Also the Spring-layer is very light on top and could be substituted with another if needed.

The server-side codebase can be divided into two parts:
- the application that can be customized for a specific need
- oskari-server as library of maven modules

### Backend architecture

The Java webapp archive (war-file) for Oskari server is packaged as oskari-map.war in [sample-server-extension](https://github.com/oskariorg/sample-server-extension) (the `webapp-map` module).

It handles most of the server side functionality alone, but doesn't need to include much code as it uses the Maven modules from `oskari-server` that handle most of the things it needs.

You can find Oskari-server source code [here](https://github.com/oskariorg/oskari-server).

The webapp is extensible and you can add more modules from oskari-server or remove ones you don't need to adjust the functionalities your app requires. It is also very easy to add more handlers/controllers for any application specific needs when creating your own geoportal/web mapping application.

The server application template has Maven modules for an example setup with:

- `app-resources` (has initial database data and migrations for the application)
- `app-specific-code` (has a "Hello World" request/action handler as an example of app specific code)
- `webapp-map` (uses the other two modules and packages everything up in a Java war-file)


### Server-side libraries and technologies

Oskari backend uses the following libraries and technologies:

* Tomcat/Jetty or similar as Java servlet container
* PostgreSQL (database)
* PostGIS (spatial data extension for PostgreSQL)
* Redis (for caching and communication in clustered server environment)

Based on your needs you can decorate your architecture by adding components like these in front of the Oskari server:

* HAProxy (proxy, load balancer)
* Apache HTTPD (proxy, load balancer)
* Nginx (proxy, load balancer)
* F5 load balancer

Having HTTPD or nginx for serving the static frontend application and passing other requests to Tomcat/Jetty is a popular choice.

### Server-side source code

You can find Oskari backend source code in [here](https://github.com/oskariorg/oskari-server).

Note that oskari-server doesn't have a runnable webapp to reduce forking as webapps are usually (heavily) customized per application requirements.

You can find a template to start your server customization with our example template [here](https://github.com/oskariorg/sample-server-extension).
