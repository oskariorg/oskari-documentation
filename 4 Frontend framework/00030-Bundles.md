## Bundles

A bundle can be considered as a building block in an Oskari application. A bundle provide some documented functionality and optionally an API that it offers to other bundles for interaction purposes. A bundle has an `id` that is used to document the functionality and the API, but can have multiple parallel implementations with the idea that switching between implementations is a drop-in replacement. An example of this could be a 2D (OpenLayers) and a 3D (Cesium) implementation of the map functionality (`mapmodule`). They both integrate with the rest of the application by providing and using the same documented API, but offer a very different experience for the end-user.

The implementation details of a bundle should not really matter, but the API and the functionality they provide should be documented and either be backwards compatible or any changes need to be documented in the changelog.

A comprehensive versioned documentation of all available bundles can be found [here](https://oskari.org/documentation/api/bundles/latest/)

### Bundle architecture

Starting with 1.48.0 Oskari supports writing bundles in JavasScript ES6. This update allows many improvements to the way Oskari bundles are composed.

![bundle.png](../resources/images/bundle.png)

**Service**
It's the Service's responsibility keep the state related to the bundle business logic consistent and updated. Possibly saving this state to the backend via action routes when needed. The Service exposes public methods to mutate the state and allows other components within the bundle to register for notifications about state changes.

**View**
It's the View's responsibility to update the Flyout/Tile DOM accordingly when it receives notification from the Service that state has changed. It's also the View's responsibility to mutate the Service as a reaction to user input. Flyouts, Tiles, Popups etc. are parts of the View.

**Map Plugin**
It's the Map Plugin's responsibility to update the map related presentation when it receives notification from the Service that state has changed. If the bundle does not have map related functionality, it doesn't need to implement a Map Plugin. Map layers, map controls, map interactions are implemented by Map Plugins.

**Data flow**
User input -> View/Plugin mutates Service by calling a public method on the Service -> Service updates internal state (possibly saving to backend) -> Service notifies all interested components by triggering an event -> Listening components (Views & Map Plugins) update their presentation.

**Communication between bundles**
If a bundle wants to allow other bundles to interact with itself, the bundle can register requests and publish events to Oskari sandbox.

A Service can also be exposed to other bundles with a call to sandbox.registerService(service). Afterwards other bundles can obtain a reference to the service by calling sandbox.getService(serviceName).

**External dependencies**
If you bundle depends on external library code, the libary must be referenced to be included into the build.

If the library is a part of oskari-frontend repository (lodash, d3, etc.), or generally if the library is distributed a separate JS file you should reference it in your bundle.js.

If you want to use libraries distributes as NPM modules, you can npm install --save them and import as usual. But check first that the library isn't in use under oskari-frontend libraries/ to avoid duplication of library code.

### Bundle manager/loader

- loads Bundle Definitions
- manages bundle state and lifecycle
- loads Bundle JavaScript sources and CSS resources
- instantiates Bundle Instances
- manages Bundle Instance lifecycle

The code for Oskari class system/bundle manager/Oskari loader can be found in Oskari/src. The minified version of this is available in Oskari/bundles/bundle.js and a new version of this core-functionality can be built by running npm run core command in Oskari/tools folder.
