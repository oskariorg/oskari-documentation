## Frontend

The user interface for Oskari-based services usually is a Javascript-based single-page app. The UI is built by selecting a series of bundles that provide functionalities/capabilities for an application. You can mix and match the bundles or create new ones to customize the application for your needs.

Bundles are used as uniform containers to ship and share new functionality to the application setups. Additions to an existing functionality can be implemented as plugins shipped within the bundles.

A bundle can work "as is" for providing its functionality with its own user interface and/or it can provide a documented API that can be used to interact with the functionality programmatically. One example of a bundle that doesn't have an UI itself would be a bundle called `drawtools` that only provides an API that is used by measurement tools, my places functionality and others that has the user "draw" something on the map. The API also helps implementing a customized drop-in replacements for functionalities when required.

**Bundle functionality**

* Bundles can provide an API for other bundles to request some operation through a request handler.
* A bundle can provide a request class and register a handler for the request in the Oskari framework. This is refered to as the request API for the bundle and should be fairly stable.
* Another bundle can then send the request which will be processed by the other bundle.
* Another way to communicate with other bundles is to send out an event through Oskari framework.
* Any bundle registered as an eventlistener for the given event is then notified about the event.

### Frontend source code and folder structure

The frontend for Oskari-based applications can be divided into two (or more) parts:
- the application that can be customized for a specific need
- `oskari-frontend` that provides the frontend framework, built-in UI-component library and bundles that can be used as building blocks when creating applications.
- you can also use bundles from `oskari-frontend-contrib` repository like ones from oskari-frontend and/or another third party repository

You can find an Oskari-based sample application source code [here](https://github.com/oskariorg/sample-application).

The sample application frontend source code has the following folder structure:
```
/applications - References to bundles that will be used in a specific application
/bundles - Implementation for application specific bundles
```
The applications folder has for example `geoportal` and `embedded` as applications as both of these use different set of bundles.
 For the sample-application the `geoportal` is what you expect to see when opening the Oskari application and lists all the bundles that will be used on different `geoportal` views on the example application (https://demo.oskari.org etc).
 The `embedded` application references the bundles that users can select to be included when publishing maps from the geoportal. We need to reference any bundle that could be started on an embedded map, but we can still optimize by using the `oskari-bundle` loader for ones that are always used (like the map) and use `oskari-lazy-bundle` to reference ones that see less use (like thematic maps). This way the file that end-users need to download when opening the map only has the ones that are common to get a smaller file size while also enabling users to use the more specific functionalities that are only loaded when used on the embedded maps.

Applications can and usually do use many of the bundles provided in `oskari-frontend`. You can find a list of these bundles in the [bundle documentation](/documentation/api/bundles). If you want to learn more about about [oskari-frontend](https://github.com/oskariorg/oskari-frontend) in general, you will find documentation for it under the "[Frontend framework](../4 Frontend framework/00010-ApplicationFunctionality.md)" section of the documentation.
