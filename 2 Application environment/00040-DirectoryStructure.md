## Frontend bundle implementation convention

**The folder structure** follows a pattern where the first folder under the base `bundles` folder is a `namespace` folder. Application specific bundles can choose to skip the namespace especially if all bundles of the application would be under the same namespace. Oskari uses `framework`, `mapping` and `admin` namespace for most bundles. The next folder is named after the `<bundle-identifier>`.

Any events, requests (and request handlers) and services a bundle implements should be separated into subfolders under the bundles implementation. However this is a convention, not a requirement for application specific bundles. In addition if you have divided the code into views or components that are shown on the flyout, you can create subfolders for them as well. Having an `index.js` file as the starting point for bundle definition is a nice way of shortening the reference to the bundle in applications main.js file.

There is a functional requirement when using the `oskari-bundle` loader to load your bundle that localization files should be under `resources/locale` folder relative to the `index.js` (bundle definition/instance factory file). Usually the files are named after the language code for the localization, but this is not a functional requirement. The contents of the file declares the locale for localization.

    <your root dir>
    |--bundles
       |--<mynamespace>
         |--<bundle-identifier>
            |--component
            |  |--MyComponent.js
            |--event
            |  |--MyEvent.js
            |--request
            |  |--MyRequest.js
            |  |--MyRequestHandler.js
            |--resources
            |  |--locale
            |    |--en.js
            |    |--fi.js
            |    |--sv.js
            |--service
            |  |--MyService.js
            |--view
            |  |--MyLoggedInView.js
            |  |--MyGuestView.js
            |--index.js
            |--instance.js
            |--Tile.js
            |--Flyout.js