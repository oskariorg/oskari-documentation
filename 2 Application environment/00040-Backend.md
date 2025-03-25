## Backend

Backend functionality of the platform is implemented with Controllers from Spring framework and can be easily extended to handle new functionality. Also the Spring-layer is very light on top and could be substituted with another if needed.

The server-side codebase can be divided into two parts:
- the application that can be customized for a specific need
- oskari-server as library of maven modules

### Backend architecture

The Java webapp archive (war-file) for Oskari server is packaged as oskari-map.war in [sample-server-extension](https://github.com/oskariorg/sample-server-extension) (the `webapp-map` module).

It handles most of the server side functionality alone, but doesn't need to include much code as it uses the Maven modules from `oskari-server` that handle most of the things it needs.

You can find Oskari-server source code [here](https://github.com/oskariorg/oskari-server).

The webapp is extensible and you can add more modules from oskari-server or remove ones you don't need to adjust the functionalities your app requires. It is also very easy to add more handlers/controllers for any application specific needs when creating your own geoportal/web mapping application.

The server application template has Maven modules for an example setup with:

- app-resources (has initial database data and migrations for the application)
- app-specific-code (has a "Hello World" request/action handler as an example of app specific code)
- webapp-map (uses the other two modules and packages everything up in a Java war-file)

The backend architecture in oskari-server Maven-modules can be divided into three layers: service layer, control layer and interface layer:
1) The interface layer is very light with Spring framework Controllers for handling requests and can be easily substituted to run as portlets or similar.
2) The controllers pass concrete HTTP-requests on to Oskari control-modules that can further process the requests and write responses.
3) Services are used by the control-modules to handle business-logic. The service-modules could (in theory) be used in any Java-based software as libraries.

Oskari uses a concept of "action route" for processing requests made by the frontend application. Requests for action routes are processed like this:

```mermaid
flowchart TD
    A>User] -->|Opens page| S
    X>User] -->|Makes a search| S
    PU>User] -->|Publishes a map| S
    S[ActionRouteController] --> C{ActionRoute}
    C -->|Load page| D[GetAppSetupHandler]
    C -->|Publish a map| P[AppSetupHandler]
    C -->|Search results| E[SearchHandler]
    C -->|Application specific action| F[Your code]
    D -->|Load appsetup| AppSetupService(AppSetupService)
    P -->|Create appsetup| AppSetupService(AppSetupService)
    E -->|Search| SS(SearchService)
    SS --> |Search|OpenStreetMap>OpenStreetMap]
    SS --> |Search|WFS-service>WFS-service]
    AppSetupService --> dbId[("oskaridb")]
```

**Service layer**

Service modules should be common libraries usable in any application. The actual business logic for Oskari operations should be in these modules.

* service-base has some common helpers and domain classes which are used throughout Oskari backend
* service-permission is a generic authorization lib for deciding who gets to see/do what
* service-search is a generic search lib that can be extended by adding and registering search channels.
* service-map has most (maybe a bit too much) of the business logic for servicing the Oskari functionalities
* service-control defines the control/routing structures/interfaces for control-layer to build upon
* shared-test-resources has some common helpers/templates to help testing

**Control layer**

Control modules build on top of the service layer.

* control-base is the basis for all control-modules and has most of the basic request handlers needed by the Oskari frontend.
	* NOTE! control-base contains some very specific functionalities that should be separated into separate control-extensions (for example thematic maps support)
* control-myplaces provides funtionality related to myplaces functionality.
* control-example provides example implementations for functionalities required by Oskari but usually overridden by platform specific functionalities such content management for user guide etc.
* content-resources has tools, templates and scripts for populating and migrating the database

Functions:
* Handles requests made by Oskari frontend
* Parses request parameters for input values to be used on service invocations
* calls one or more services to do business logic
* format a response based on service response

**Interface layer**

The interface-modules build on top of the control-modules. Basically an HTTP interface with reference implementations for:

* HTTP Servlet: oskari-server/servlet-map
* Webapp: sample-server-extension/webapp-map

Responsible for:

* Handling user sessions
* Generating an ActionParameters object based on incoming request abstracting/normalizing the request for control layer
* Forwarding the request to control layer

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
