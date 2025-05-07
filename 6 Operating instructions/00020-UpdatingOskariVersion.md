## Updating Oskari version

This section guides you through updating your Oskari instance.

We are updating the frontend and server versions **at the same time** and for the best compatibility **it's best to use same version in both**. Hotfix/patch versions might not always match but with version `major.minor.patch` the major and minor versions should match.

Before updating, the following links are recommended to at least skim through:

- [Migration Guide](https://github.com/oskariorg/oskari-server/blob/master/MigrationGuide.md)
- [Server release notes](https://github.com/oskariorg/oskari-server/blob/master/ReleaseNotes.md)
- [Frontend release notes](https://github.com/oskariorg/oskari-frontend/blob/master/ReleaseNotes.md)

### Migration and release notes

In most cases updating Oskari version means:
1. updating the version of Oskari dependency for the frontend and server and 
2. building new versions of your frontend app/server
 
Sometimes there are additional things that need to be done when updating. These are documented on [the Migration Guide in oskari-server repository](https://github.com/oskariorg/oskari-server/blob/master/MigrationGuide.md).

There can be also application spesific things that need to be taken into account when updating. For example, your application code might not compile after updating if you have changed some class in oskari-server that you are using directly in your app.

You should also at least skim through [the Release notes](https://oskari.org/documentation/docs/latest/12-Changelog) for both the server and the frontend to see if there is something that might affect your app. There might be, for example, a new implementation of a bundle/functionality that you want to use instead of the old one.

It's also a good idea to see any PRs/changes for the sample application to see if there are things that you can streamline/change in your application for the newer version:

- https://github.com/oskariorg/sample-application
- https://github.com/oskariorg/sample-server-extension

**Note!** The idea is _NOT_ to you use the sample-application/server directly but to use it as a template to create your own app/server. For example the database migrations in server-template assume they are run on an empty database and might not work properly if copied directly to your server code. We change the sample migrations between versions to make the most sense as simple examples for the latest version while actual apps (copies of sample-server) should work to migrate the existing database of the app.

### Updating the server

Updating an Oskari-powered server that is based on the [sample-server-extension](https://github.com/oskariorg/sample-server-extension) template is done by updating the value of `oskari.version` property in [pom.xml](https://github.com/oskariorg/sample-server-extension/blob/2.0.0/pom.xml#L13) file:

```xml
<properties>
    <oskari.version>1.0.0</oskari.version>
    ...
</properties>
```

To the new version:

```xml
<properties>
    <oskari.version>1.0.1</oskari.version>
    ...
</properties>
```

After this run `mvn clean install` to generate a new `oskari-map.war` under `{your.server.repository.root}/webapp-map/target`. Replace the `oskari-map.war` with the new one as described on [Build customized server-side application](../2%20Setup%20instructions/00040-BuildCustomizedServerApplication.md).

### Updating the frontend

Updating an Oskari application that is based on the [sample-application](https://github.com/oskariorg/sample-application) template is fairly simple as well. For the frontend the `oskari-frontend` (and possible `oskari-frontend-contrib`) dependency is updated by changing the version number in [package.json](https://github.com/oskariorg/sample-application/blob/2.0.0/package.json#L9) file from the current version in the app:

```
"dependencies": {
  "oskari-frontend": "https://git@github.com/oskariorg/oskari-frontend.git#1.0.0"
},
```

To the new version:

```
"dependencies": {
  "oskari-frontend": "https://git@github.com/oskariorg/oskari-frontend.git#1.0.1"
},
```

When updating Oskari version, feel free to change the application version as well in [package.json](https://github.com/oskariorg/sample-application/blob/2.0.0/package.json#L3) of your application to signal that the application has been updated.

After this you need to run `npm install` to install any new/changed libraries and `npm run build` to generate a new build under `{your.server.repository.root}/dist/[version on package.json]` as described on [Build customized frontend application](../2%20Setup%20instructions/00050-BuildCustomizedFrontendApplication.md).

Finally, remember to update the newly built frontend version to `oskari-ext.properties` under `oskari-server/lib`:

```properties
oskari.client.version=dist/[version from your package.json]
```
