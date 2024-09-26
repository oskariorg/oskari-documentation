## Basic concepts

Here are listed the explanations for some of the basic Oskari concepts and Oskari-spesific terms. If you didn't find the term you were looking for from this section, please let us know by creating an issue in GitHub.

- Application = Provides a service for the end-user
- Application-frontend = The parts of an application that are run on the browser. Combines pre-existing bundles from Oskari-frontend and application specific bundles to provide the user-interface for the service.
- Application-server = Provides backend functionality that enables the frontend to do its things.
- Bundle = A frontend code package that provides a specific functionality for the application. Basically a frontend implementation of some functionality. Usually referred to with its **id** like `search`.
    - Same functionality can have multiple implementations. If the API of implementations match they can share the same id. However the same functionality can be provided by bundles with different id's if either their API doesn't match or they have different use cases. For example the search UI is implemented on the bundle `search` with a window-like UI (see Tile/Flyout) but it's also provided as part of the mapmodule bundle as a small on-map text field.
    - Sometimes refered also as modules.
    - Bundles can provide an API of requests, events and services allowing other functionalities to interact with them.
    - Bundles have a lifecycle API with functions that are called when the functionality is started or stopped etc.

- Class = Oskari-frontend uses custom implementation of classes (defined with `Oskari.clazz.define()`). These will eventually disappear from the frontend.
- Component = Oskari provides UI components like buttons, text fields etc with a purpose of making the user interface look coherent with same kind of UI elements implemented in the same way.
- Feature = Currently used to refer vector features (points, lines, polygons on map in vector format). Generally not used anymore in Oskari documentation, but sometimes a synonym with functionality.
- Module = See Bundle. We sometimes mix our terms.
- Plugin = Bundles can contain plugins. They can be considered as mini-bundles that enhance the functionality of a bundle. Mostly used with the map module to add functionality for the map.
- RPC = Remote procedure call. In Oskari RPC refers to one of the key features of Oskari, embedding a map to another website. RPC functionality allows a developer to control the embedded map and react to events on the map from the embedding website. **The instance that hosts the embedded map and the selections made in the publisher functionality affects what can be done with RPC. Some functionalities can be disabled or not supported depending on the Oskari instance.**
- Sample application = The sample (=template) repositories are meant to be used as starting points for creating an Oskari-based service with your own selection of functionalities, data, users, layout, colors and other customizations.