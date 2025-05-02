### Oskari instance initial applications

The initial applications can be described as JSON-files on the server code and inserted to the database with <a href="https://github.com/oskariorg/sample-server-extension/blob/2.0.0/app-resources/src/main/java/flyway/app/V1_0_3__initial_db_content.java#L11" target="_blank">Flyway-migrations</a>. You can see these on the <a href="https://github.com/oskariorg/sample-server-extension/blob/2.0.0/app-resources/src/main/resources/json/apps/geoportal-3857.json" target="_blank">sample-server-extension</a> that you can use as a template to build your own Oskari instance.

More information about Flyway and migrations:
- https://flywaydb.org/
- [Upgrading](../4%20Server%20application/00040-DatabaseMigrations.md)
- [Migration scripts](../4%20Server%20application/00041-AddingDatabaseMigrations.md)

The application specific migrations can of course be done however you want but this is a built-in option to use for migrating the database and can be used to modify the content on the database like adding or removing bundles from `oskari_appsetup_bundles`. Notice that these migrations are only run once so when it has been run, modifying the code doesn't get the migration run again. You will need to delete rows on the `oskari_status_[your apps module]` to the migrations to run again on a given database.

### Modifying applications after the initial database population

Here's an example Flyway migration that you can add on your server-extension code.

Note that the order in which migrations are run is determined by the version number on the filename (the below would be 1.0.0). When you want to add a new migration after one has been run, you need to bump the version to a higher number like 1.0.1, 1.1.0, 2.0.0. So take a note a consider a versioning strategy for your Oskari instance. For best practices: the version on the frontend codes `package.json`, the version on the server Maven modules `pom.xml` and the version on the Flyway migration should be tied to each other some way. 

Open the folder `sample-server-extension/app-resources/src/main/java/flyway/app`. Inside, you will find various files that can be used as templates for your flyway migration.

The code in each file begins with `package flyway.app;`

The Flyway module `app` on our `sample-server-extension` repository can be changed to something that describes your app (`custom_flyway_module`). Note that you can also have multiple Flyway-modules on your app. The modules that are used when the server runs are set on `oskari-ext.properties` in oskari-server as `db.additional.modules`.

The migration name is after two underscores after the version. This can be anything you like on your application but it's easier to maintain if naming makes sense with the migration that is being run.

For removing bundles after they have been added, you can see the other methods available on the `AppSetupHelper` class. You also don't need to use these helpers and can modify the tables with for example SQL. These are just common operations so we have helpers for them to make it as easy as it can be.
