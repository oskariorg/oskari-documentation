## Framework and concepts

Oskari provides tools for easily building multipurpose web mapping applications utilizing distributed Spatial Data Infrastructures like INSPIRE. The Oskari web application includes frontend code for browser-based user interface and a backend codebase that is run on the server. The user interface is implemented in JavaScript and the server functionality in Java. Both, the frontend and the backend, are built with extensibility in mind.

A example for building your own service is provided as a template application that can be run as is and it enables an easy starting point for customizing the service.

The picture below shows the generic idea of Oskari.

![application-environment-1.png](../resources/images/oskari-generic.drawio.png)

### Concepts

- Application = Provides a service for end-user
- Application-frontend = Oskari-frontend with collection of pre-existing bundles and application specific bundles.
- Application-server = Provides backend functionality that enables the frontend to do it's things.

- Bundle = A component in Oskari application that provides some functionality for the service. Usually referred to with its id like 'search'. Basically the frontend application is a collection of bundles that together form the application.

 A selection of Oskari classes which form a component that offers additional functionality for an application. A bundle can offer multiple implementations for a functionality which can then be divided into smaller packages for different application setups. Packages can be used to offer a multiple views for the same functionality for example search functionality as a small on-map textfield or a window-like UI (see Tile/Flyout) for the same functionality.

- Class = Oskari-frontend uses custom implementation of classes (defined with Oskari.clazz.define()). These will eventually disappear from the frontend.
- Plugin = bundles can contain plugins. They can be considered as mini-bundles that enhance the functionality of a bundle. Mostly used with the mapmodule to add functionality for the map.
- Module = See Bundle. Modules are components that can be registered to Oskari. Registering allows these components to for example send requests and receive events. Registering a module also calls the module init method. Usually modules start listening to events and register request handlers on start methods. stop method is called when the module is stopped but they usually aren't stopped.

- Feature = not used anymore, sometimes a synonym with functionality but currently used to refer vector features (points, lines, polygons on map in vector format)
- Component = Mainly used to refer a React-component? Sometimes used to refer a jQuery component like a common implementation for a button or such.