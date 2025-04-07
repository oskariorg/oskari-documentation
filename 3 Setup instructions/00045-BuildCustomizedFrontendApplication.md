## Build customized frontend application

This section describes how to replace the prebuilt frontend application with your own version.
This is the intended way of customizing Oskari-based applications.

### Frontend requirements

The following items are required for the development process:

* [NodeJS](https://nodejs.org/en) (developed using 18+)
* [Git client](https://git-scm.com/) (optional)
* [GitHub account](https://github.com/) (optional)

You will need an environment to run the code in as described on [Setup application server](00030-SetupApplicationServer.md)

Feel free to use the [git conventions](../8 Developing instructions/00115-GitGuidelines.md) used in Oskari development with your own customizations, but it's your app so you can make your own choices.

### Create your frontend application repository

You can use our `sample-application` template for creating a repository that will have your application customizations under your own GitHub user/organization:
https://github.com/new?template_name=sample-application&template_owner=oskariorg

Clone the repository to your own computer with git. You can use any program you are comfortable with to work with git, but on command line you can run:

```sh
git clone https://github.com/oskariorg/sample-application.git
```
Replace `oskariorg` with your own username/organization name and `sample-application` if you changed the repository name for your app.
For simple testing you can also just clone our template repository for tinkering, replacing the existing `sample-application` folder with your cloned repository.

If you don't use git you can also just download the repository code as zip from [GitHub](https://github.com/oskariorg/sample-application/archive/refs/heads/master.zip).

Note! The sample application, including its source code and build, is available in the `sample-application` folder within the [Oskari download package](/download). The frontend application builds are generated in the `dist` folder under the `sample-application` and the prebuilt version is included in the download zip file.

### Build your version of the frontend application

Modern web applications are developed using JavaScript syntax that is not fully supported by older browsers and the code needs to be processed before it can be used by the end-user browsers. This step also includes bundling and minifying the code so it's more compact for consumption than the human-friendly version that is used for development. This will reduce the startup time for the end-user dramatically.

1) Go to the root folder of the repository (`cd sample-application`)
2) On the first time (or after you have updated Oskari version) run `npm install` to install dependencies
3) Run the npm command to generate new build product:

```sh
npm run build
```
This creates a new version folder under `sample-application/dist/`. The version is named after `version` in the `package.json` file (having package.json version as `1.0` will generate `sample-application/dist/1.0/`).

### Frontend location and version

If you want to use another name than sample-application you can change the folder in which the application server looks for the frontend.
For example you might want to use some other name for the repository that holds your version of the frontend application.
 This can be changed in `oskari-server/conf/Catalina/localhost/Oskari.xml`
([https://github.com/oskariorg/sample-configs/blob/master/tomcat-10/oskari-server/conf/Catalina/localhost/Oskari.xml](here)) by changing the value of `docBase`:

```xml
<Context docBase="../../sample-application" reloadable="true" />
```
Note that the frontend code doesn't need to be server through the application server and could be served with servers like nginx or Apache HTTPD.
However the server does have a configuration which version of the frontend it uses on `oskari-server/lib/oskari-ext.properties` 
[https://github.com/oskariorg/sample-configs/blob/master/tomcat-10/oskari-server/lib/oskari-ext.properties](here):

```properties
oskari.client.version=dist/2.0.0
```

Where the version in `oskari-ext.properties` must match the folder where the frontend app build product was generated to (`package.json` version).

### Simple frontend modifications and dev-server

An easy way of seeing a `Hello world` type of modification is adding something like `console.log("Hello world")` in the `start()` function of the [SampleInfoBundleInstance.js](https://github.com/oskariorg/sample-application/blob/master/bundles/sample-info/SampleInfoBundleInstance.js).

Unfortunately any change to the frontend requires a build to be run to see the changes updated. To help with this the frontend offers the option to use Webpack dev-server by running `npm run start` instead of `npm run build`.
The dev-server starts serving the frontend files from http://localhost:8081 and requires the server to respond from http://localhost:8080. It also requires that the server is configured to use this client version in `oskari-server/lib/oskari-ext.properties` (as opposed to version from package.json):

```properties
oskari.client.version=dist/devapp
```
**Note!** Changes to `oskari-ext.properties` requires the server to restart to take effect.

The dev-server provides the changed version after automatic page reloading so it's much more convenient to use for development than running the build after every change, but it's not perfect so if you don't see your changes you might need to restart the dev-server during development.

#### Frontend build details

The build scripts work by reading a `main.js` file that links all the functionality/bundles you want to use in your application together and should match the appsetup (bundle collection) used on the website you are creating including any dynamic (role-based) bundles that are added on the fly. So basically all the bundles you want to use in that application. Here's an example for the basic [geoportal appsetup](https://github.com/oskariorg/sample-application/blob/2.0.0/applications/geoportal/main.js) and for an [embedded map](https://github.com/oskariorg/sample-application/blob/2.0.0/applications/embedded/main.js). These are both processed when running `npm run build`. Note that linking the bundles to be part of the frontend application in `main.js` only includes the functionalities that _CAN_ be used in an application. The server/database configurations dictates which of the functionalitise that have been included is actually started for a given application. This allows for example admin-bundles to be shown only when the user has the admin role, but also allows reusing the same frontend application to show different kinds of applications based on the database configuration. For example using the same `embedded` application for all different kinds of published maps where end-users can select through a browser-based UI which functionalities will be included in any given embedded map.

If you take a look at the package.json [script for build](https://github.com/oskariorg/sample-application/blob/2.0.0/package.json#L22) you can see that the parameter `--env appdef=applications` is used to point the build to search for main.js files under the `applications` folder. You can change this in your own app as you wish.

The best point to start customizing your app is changing the `main.js` file under the sample `geoportal` application to only include the bundles that you are using. This reduces the amount of code the end-user needs to load. You can also safely remove the 3D applications if you don't need them. The `embedded` application (code for published maps) usually stays more or less the same and the `geoportal` one is usually customized based on requirements.

When changing `main.js` to include application specific bundles and/or customizing the application you need to run `npm run build` again to create new build product under the `dist` folder. You will also need to do this when updating the new version of Oskari or after changing any application specific code/bundles you want to use.
