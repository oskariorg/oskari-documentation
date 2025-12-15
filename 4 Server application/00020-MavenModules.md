## Maven modules

The backend architecture in oskari-server Maven-modules can be divided into three layers: service layer, control layer and interface layer:
1) The interface layer is very light with Spring Framework Controllers for handling requests and can be easily substituted to run as portlets or similar.
2) The controllers pass concrete HTTP-requests on to Oskari control-modules that can further process the requests and write responses.
3) Services are used by the control-modules to handle business-logic. The service-modules could (in theory) be used in any Java-based software as libraries.

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
	* NOTE! control-base contains some very specific functionalities that should be separated into separate control-extensions
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

* HTTP Servlet: `oskari-server/servlet-map`
* Webapp: `sample-server-extension/webapp-map`

Responsible for:

* Handling user sessions
* Generating an ActionParameters object based on incoming request abstracting/normalizing the request for control layer
* Forwarding the request to control layer
