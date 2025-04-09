## Oskari global and sandbox

The Oskari frontend framework functionality is located in the `/src` folder in [oskari-frontend](https://github.com/oskariorg/oskari-frontend/tree/master/src).
The folder also includes React.js based UI-component library (based on AntD-components).

### Oskari global

A global `Oskari` variable is introduced for Oskari frontend applications to access the framework functionalities.

It provides some functionalities that applictions can use (bundles usually) like:
- application environment like supported languages, current language and application UI theme
- bundle registry
- sandbox registry (see below)
- bundle lifecycle handling
- tracking of current user
- access to localizations
- class system (being migrated away in favor of ES-classes)
- helper functions for colors etc
- logging functionality

### Sandbox

Sandbox is:
- event/message bus for frontend bundles to communicate through requests and events etc
- registry for modules (~bundle instances except bundle can have many "modules")
- registry for services (bundles that expose their services for other bundles to use)
- conveniency getters for map state
- functionality context

There can be multiple sandboxes in an Oskari frontend application to scope messaging, but most commonly there is just one that can be accesssed with `Oskari.getSandbox()`.
 The function takes a string-parameter enabling multiple sandbox instances to be used but it's very uncommon to use other than the default sandbox.

Bundle instances get a sandbox reference as parameter on their `start(sandbox)` function and they **should** save and use that reference.
This is handled by `BasicBundleInstance` base class that also provides `getSandbox()` function as a way of using the given sandbox.
This enables scoping the bundle interactions that could be used for having two instances of for example the mapmodule on the same page/Oskari application.

Note that some bundles might use the `Oskari` global to get a sandbox reference (effectively defeating the purpose), but this is not intended use.

```mermaid
sequenceDiagram
    participant mapmodule as mapmodule<br/>bundle
    participant layerlist as layerlist<br/>bundle

    mapmodule->>Sandbox: Register as started
    mapmodule->>Sandbox: Add handler for AddMapLayerRequest
    layerlist->>Sandbox: Register as started
    layerlist->>Sandbox: Add listener for AfterMapLayerAddEvent
    Note over layerlist: User adds a layer to map using<br/> UI provided by layerlist
    layerlist->>layerlist: Click toggle
    layerlist->>Sandbox: Send AddMapLayerRequest
    Sandbox->>mapmodule: handle AddMapLayerRequest
    mapmodule->>mapmodule: Download layer metadata<br/>and add to map
    mapmodule->>Sandbox: Trigger AfterMapLayerAddEvent
    Sandbox->>layerlist: call listener for AfterMapLayerAddEvent
    layerlist->>layerlist: update state/UI to show layer on map
```
