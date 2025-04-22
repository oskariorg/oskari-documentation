### How to add a bundle

Bundles can be considered as building blocks in an Oskari application. A bundle is a selection of JavaScript files that provide some functionality to be used as part of an application.
 A bundle can offer multiple implementations for a functionality which can then be divided into smaller packages for different application setups. 
 For more about bundles see the documentation under [Frontend framework](../3%20Frontend%20framework/00030-Bundles.md).

In order to get bundle up and running in your Oskari-based application, the bundle needs to be registered to the database and added to an appsetup you want to show it on. There are two tables where it shoud be added:

- oskari_bundle
- oskari_appsetup_bundles

`oskari_bundle` includes rows for all available bundles in the Oskari instance. Any new bundles should be added/registered here so they can be used in an appsetup. It is recommended to use Flyway-scripts when making changes to database.
 Documentation about migrations can be found [here](../4%20Server%20application/00040-DatabaseMigrations.md).

#### Registering a new bundle

If you are adding a new bundle for your application or to the built-in ones under `oskari-frontend`, you should add a migration script to register the bundle on the database so the server recognizes it and allows using it on appsetups.
 The `oskari-server` provides `org.oskari.helpers.BundleHelper` helper class for doing this and a bundle can be registered with a migration like this (Replace `<bundle-identifier>` with the bundle id and fix the migration version in the class name):

```java
package flyway.app;

import org.flywaydb.core.api.migration.BaseJavaMigration;
import org.flywaydb.core.api.migration.Context;
import org.oskari.helpers.BundleHelper;

public class VX_Y_Z__register_bundle extends BaseJavaMigration {

    @Override
	public void migrate(Context context) throws Exception {
        BundleHelper.registerBundle(context.getConnection(), "<bundle-identifier>");
	}
}
```
The Flyway migration checks that a bundle with the same id hasn't been registered before registering it.
Note that the package of the migration (`app` here) dictates the Flyway module it is included in.
 In `oskari-server` most migrations are done on the `oskari` module, but if you have an application specific bundle you should add it in your applications migration module.
 As an SQL the same can be accomplished like this, but you would need to be sure you are not adding duplicates to the table:

```sql
-- Register bundle to oskari_bundle table
INSERT
INTO oskari_bundle
(
	name,
	config,
	state
)
VALUES
(
	'<bundle-identifier>',
	'{}',
	'{}'
);
```
If you want the bundle to have a default configuration or state anytime it's added to an appsetup, you can define them when registering the bundle in `oskari_bundle` table. You can also do this with the `BundleHelper` class.

#### Adding bundle to an appsetup

After bundle has been registered to `oskari_bundle` table, it can be added to `oskari_appsetup_bundles` table to be used in appsetups.
The `oskari-server` provides `org.oskari.helpers.AppSetupHelper` helper class for doing this easily:

```java
package flyway.app;

import org.flywaydb.core.api.migration.BaseJavaMigration;
import org.flywaydb.core.api.migration.Context;
import org.oskari.helpers.AppSetupHelper;

public class VX_Y_Z__add_bundle_to_apps extends BaseJavaMigration {

    public void migrate(Context context) throws Exception {
        AppSetupHelper.addBundleToApps(context.getConnection(), "<bundle-identifier>");
    }
}
```
By default this adds the bundle to all appsetups of type `DEFAULT` (default geoportal apps) and `USER` (geoportal based appsetups saved by users).
 There are also other useful methods on `AppSetupHelper` from checking if a bundle exists on an appsetup to updating a bundles state or config on an existing appsetup.
 
Below is an example SQL for adding a bundle to `oskari_appsetup_bundles` table manually:
```sql
-- Add bundle to default geoportal appsetup
INSERT
INTO oskari_appsetup_bundles
(
	appsetup_id,
	bundle_id,
	seqno,
	config,
	state,
	bundleinstance
)
VALUES (
	(SELECT id FROM oskari_appsetup WHERE application='geoportal' AND type='DEFAULT'),
	(SELECT id FROM oskari_bundle WHERE name='<bundle-identifier>'),
	(SELECT max(seqno)+1 FROM oskari_appsetup_bundles WHERE appsetup_id=(SELECT id FROM oskari_appsetup WHERE application='geoportal' AND type='DEFAULT')),
	(SELECT config FROM oskari_bundle WHERE name='<bundle-identifier>'),
	(SELECT state FROM oskari_bundle WHERE name='<bundle-identifier>'),
	'<bundle-identifier>'
);
```

After these steps, and when bundle is defined correctly in front-end code, the bundle should be loaded when starting your map application. Great way to check if the bundle is loaded at start is to look at startupSequence in GetAppSetup in developer console. The developer console will also warn if the server response references a bundle to be started as part of the application, but the javascript files for that bundle has not been included in the frontend application.
