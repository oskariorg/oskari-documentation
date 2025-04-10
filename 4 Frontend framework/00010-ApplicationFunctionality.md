# Frontend framework

This documentation section contains documentation for the frontend framework that Oskari frontend provides and what it means for applications that are built using [oskari-frontend](https://github.com/oskariorg/oskari-frontend).

Oskari frontend source code has the following folder structure:
```
/api - The documentation of bundles and APIs they provide with a change log of changes to the API
/bundles - Implementation files for built-in bundles
/packages - Legacy-definition files for bundles (content is being migrated to bundles).
/resources - Common CSS styles/images
/src - Code for Oskari framework
/tools - Random templates and scripts for generating CSV-files based on localization
/webpack - Helpers and configurations for current build tools
/libraries - Older jQuery plugins and other dependencies/libraries that are not reasonably available through npm
```
The main folders are:
- bundles (functionality implementations you can use in applications)
- src (for framework code)
- webpack (for build scripts)

 Note! We have started migrating the contents under `packages` folder to `bundles` with a newer format. These two formats require different Webpack-loaders when referenced from the application `main.js` file.

The folder structure for `bundles` follows a pattern where the first folder under the base folder is a `namespace` folder. Oskari uses `framework` and `mapping` namespaces for most of the bundles and `admin` for admin tools. The namespace is purely cosmetic and is for grouping the bundles/organizational purposes. When creating app-specific bundles you don't have to use a namespace but you can if you wish. The next folder after the namespace is named after the `{bundle-identifier}`. Note that under the `packages` folder there can be a folder with the name `bundle` in between.
