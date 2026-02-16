## How to enable WFS-T functionality (Content editor)

The user-interface is provided as an Oskari bundle `content-editor` in https://github.com/oskariorg/oskari-frontend-contrib/tree/master/bundles/tampere/content-editor.

Extends Oskari functionality to support editing of wfs layer features and feature geometries.

### Configuration

Add new permission type to `oskari-ext.properties`. You can also add localized labels for the permission to make it easier for admins to know what the permission type is about:

```properties
permission.types = EDIT_LAYER_CONTENT
permission.EDIT_LAYER_CONTENT.name.fi=Muokkaa tasoa
permission.EDIT_LAYER_CONTENT.name.en=Edit layer
```

Add bundle dynamically to roles in `oskari-ext.properties` similar to admin functionalities. For example adding it for Admin could be done like this (you can use a different role/roles for editing the features):

```properties
actionhandler.GetAppSetup.dynamic.bundles = ..., content-editor
actionhandler.GetAppSetup.dynamic.bundle.content-editor.roles = Admin
```

Include the Oskari bundle in your application `main.js`:

```javascript
import 'oskari-lazy-bundle?content-editor!oskari-frontend-contrib/packages/tampere/bundle/content-editor/bundle.js';
```

### Allowing editing for a user role

After configuring the additional permission type (above) for your Oskari-based service, you should see the new permission type on the usual layer administration user-interfaces for managing layer access in Oskari (for example the `admin-layereditor` and `admin-permissions` UIs). 

Use your preferred access management user-interface to grant `Edit layer` permission (see localization example above) for a layer where users should be able to edit the features.

Note! The role you give the permission to should match the one that you configured the bundle to be started for in the `oskari-ext.properties`.
Note 2! Permission to edit can be granted for any WFS-layer, but the service must support the Transaction operation (WFS-T) for the actual saving/deleting of features to be successful.
Note 3! Layers with dynamic ids (ids generated at runtime) can't be edited as the backing service can't identify the feature by an id it generated on-the-fly.

### Using the content-editor

Add the layer on the map in Oskari and the 'feature editor' functionality can now be accessed through the selected layers listing by selecting it from the tools for that layer (the `...` menu for that layer).

### Known issues

#### Coordinate reference systems

Editing is only available for geometry in `EPSG:3067` CRS for time being (transforms are missing, but editing in the native CRS of the Oskari-service should work).
Setting SRID for features in the PostgreSQL database of the WFS-service before editing, if it is 0 (default):

```sql
UPDATE [table] SET <geometry>=ST_SetSRID([geometry field],3067)
```
WFS layer doesn't work e.g. for MapClick, if there are mixed SRID in the database table.
Mixed Srid could be checked with below sql:

```sql
SELECT distinct(ST_SRID([geometry field])) as srid, count(*) from [table] group by srid;
```

#### Geometry types

Geometry type of geometry-column must be `Geometry`, `MultiPoint`, `MultiLineString` or `MultiPolygon`.
in Postgres wfs-t edit table (layer) for time being.

On the other words only `MultiPoint`, `MultiLineString` or `MultiPolygon` geometries are supported in Oskari feature edit (wfs-t).
