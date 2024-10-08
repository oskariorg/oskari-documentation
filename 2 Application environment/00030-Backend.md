## Backend

Backend functionality of the platform is implemented as a Java servlet, which can also be extended to handle new functionality.

### Backend architecture

Oskari backend architecture depicted:

![oskari_architecture_backend.png](../resources/images/oskari_architecture_backend.png)

Backend architecture can be divided into three layers: service layer, control layer and interface layer. Webapp for Oskari server is packaged as oskari-map.war in sample-server-extension.

It handles most of the server side functionality alone. The webapp is extensible and can be compiled from oskari-server components and built on them to create your own geoportal/web mapping server.

The backend is layered as services, controls, interfaces (though only webapp-map uses this extensively at the moment).

![components.png](../resources/images/components.png)

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

![Service.png](../resources/images/Service.png)

* control-base is the basis for all control-modules and has most of the basic AJAX handlers needed by the Oskari frontend.
	* NOTE! control-base contains some very specific functionalities that should be separated into separate control-extensions (for example thematic maps support)
* control-myplaces has AJAX funtionality related to myplaces functionality.
* control-example has example implementations for AJAX functionalities required by Oskari but usually overridden by platform specific functionalities such as user and content management.
* content-resources has tools, templates and scripts for populating and upgrading the database

Functions:
* Handles AJAX requests made by Oskari frontend
* Parses request parameters for input values to be used on service invocations (ActionParameters is not )
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

### Libraries and technologies

Oskari backend uses the following libraries and technologies:

* HAProxy (proxy, load balancer)
* Apache (proxy, load balancer)
* Nginx (proxy, load balancer)
* F5 load balancer
* Jetty
* Tomcat
* PostgreSQL (database)
* PostGIS (spatial database)

### Source code and folder structure

You can find Oskari backend source code in [here](https://github.com/oskariorg/oskari-server).

Oskari backend source code has the following folder structure:

The folder structure follows a pattern where the first folder under the base folder is a namespace folder. Oskari uses framework for the main bundles, but this is optional and you can separate your bundles to own namespace. The next folder in the structure is named bundle. This is just a convention and is not a functional requirement. The next folder is named after the `<bundle-identifier>`.