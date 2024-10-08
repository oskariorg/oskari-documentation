## Bundles

A bundle is a component in an Oskari application. A bundle is a selection of Oskari classes which form a component that offers additional functionality for an application. A bundle can offer multiple implementations for a functionality which can then be divided into smaller packages for different application setups. Packages can be used to offer a multiple views for the same functionality for example search functionality as a small on-map textfield or a window-like UI (see Tile/Flyout) for the same functionality.

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
