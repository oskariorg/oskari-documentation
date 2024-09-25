## Basic concepts

Here are listed the explanations for some of the basic Oskari concepts and Oskari-spesific terms. If you didn't find the term you were looking for from this section, please let us know by creating an issue in GitHub.

- Application = Provides a service for the end-user
- Application-frontend = Oskari-frontend with collection of pre-existing bundles and application specific bundles.
- Application-server = Provides backend functionality that enables the frontend to do its things.

- Bundle = A component in Oskari application that provides some functionality for the service. Usually referred to with its **id** like 'search'. Basically the frontend application is a collection of bundles that together form the application.
    - Bundle can consist of a selection of Oskari classes which together form a component that offers additional functionality for an application. A bundle can offer multiple implementations for a functionality which can then be divided into smaller packages for different application setups. Packages can be used to offer a multiple views for the same functionality - for example search functionality as a small on-map text field or a window-like UI (see Tile/Flyout).

- Class = Oskari-frontend uses custom implementation of classes (defined with `Oskari.clazz.define()`). These will eventually disappear from the frontend.
- Component = Mainly used to refer a React-component? Sometimes used to refer a jQuery component like a common implementation for a button or such.
- Feature = Currently used to refer vector features (points, lines, polygons on map in vector format). Generally not used anymore in Oskari documentation, but sometimes a synonym with functionality.
- Module = See Bundle. Modules are components that can be registered to Oskari. Registering allows these components, for example, to send requests and receive events. Registering a module also calls the module init method. Usually modules start listening to events and register request handlers on start methods. Stop method is called when the module is stopped but they usually aren't stopped.
- Plugin = Bundles can contain plugins. They can be considered as mini-bundles that enhance the functionality of a bundle. Mostly used with the map module to add functionality for the map.
- RPC = Remote procedure call. In Oskari RPC refers to one of the key features of Oskari, embedding a map to another website. RPC functionality allows a developer to control the embedded map and react to events on the map from the embedding website. **The instance that hosts the embedded map and the selections made in the publisher functionality affects what can be done with RPC. Some functionalities can be disabled or not supported depending on the Oskari instance.**
- Sample application = The sample (=template) repositories are meant to be used as starting points for creating an Oskari-based service with your own selection of functionalities, data, users, layout, colors and other customizations.