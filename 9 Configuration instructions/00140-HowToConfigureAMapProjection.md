## How to configure a map projection

In Oskari application it is possible to configure different map projection for each view/appsetup. The map projection is configured for the mapfull bundle. Bundle configurations are in JSON-format that can be found in the database table *portti_view_bundle_seq* in column *config* where *bundle_id* matches the bundles id in portti_bundle table. If there are many views in the database, there are also many mapfull configurations.

To get all the configurations for the "mapfull" bundle from database run this script:

```sql
SELECT view_id, config FROM portti_view_bundle_seq WHERE bundle_id IN (SELECT id FROM portti_bundle WHERE name='mapfull')
```
Select the view_id that matches the view you want to configure (default view/publish template probably) and get the 'config' for that row. The default views are configured in oskari-ext.properties or if there is no configuration, the publish template is a view with type 'PUBLISH' and the default view with type 'DEFAULT'. Notice that if you have an old database with personalized default views you will also need to update all views. For this we recommend doing a Flyway-script.

oskari-ext.properties under {jetty.home}/resources:
```
# this is the publish template id
view.template.publish=3

# this is the generic default view id
view.default=4

# These are role specific default views
view.default.Admin=4
view.default.Guest=1
view.default.User=3

# This is the order in which user roles are matched to default views.
view.default.roles=Admin, User, Guest
```
SQL to get default view and publish template if not configured in oskari-ext.properties:
```sql
SELECT * from portti_view where type IN ('DEFAULT', 'PUBLISH');
```

 The config includes functional configuration for the bundle which also includes for example which features to activate (plugins). The interesting part for projection configuration is the **mapOptions** and **projectionDefs** JSON-segments. These might not be present in the config in which case Oskari uses these defaults:

```json
{
"mapOptions": {
   "maxExtent": {
      "left": 0,
      "bottom": 0,
      "right": 10000000,
      "top": 10000000
   },
   "srsName": "EPSG:3067",
   "resolutions":[2000, 1000, 500, 200, 100, 50, 20, 10, 4, 2, 1, 0.5, 0.25],
   "units": "m"
},
"projectionDefs": {
   "EPSG:3067": "+proj=utm +zone=35 +ellps=GRS80 +units=m +no_defs",
   "EPSG:4326": "+title=WGS 84 +proj=longlat +ellps=WGS84 +datum=WGS84 +no_defs"
}
... there's propably more than these in the config, but the above are projection related ...
}
```

To configure the projection you must add or edit these JSON-segments. An example fragment for EPSG:3057 based map:

```json
    {
        plugins : [ ....],
        ...,
        "mapOptions": {
           "maxExtent": {
              "left": -39925.6142,
              "bottom": -38980.4932,
              "right": 1001273.8754,
              "top": 986514.2978
           },
           "srsName": "EPSG:3057",
           "resolutions":[2048,1024,512,256,128,64,32,16,8,4,2,1,0.5,0.25]
        },
        "projectionDefs": {
           "EPSG:4326": "+title=WGS 84 +proj=longlat +ellps=WGS84 +datum=WGS84 +no_defs",
           "EPSG:3057": "+proj=lcc +lat_1=64.25 +lat_2=65.75 +lat_0=65 +lon_0=-19 +x_0=500000 +y_0=500000 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs"
        }
    }
```

**mapOptions**
- srsName is the name of the projection that should be used
- maxExtent means projected bounds of the chosen projection. These are the bounds that the map engine will allow the user to pan the map.
The bounds can be found f. e. in [spatial reference web site](http://spatialreference.org/ref/epsg/isn93-lambert-1993/).
- resolutions are the zoom levels

**projectionDefs** should include [proj4js](http://proj4js.org/) configurations for all the projections that are going to be used in the application.
The oskari-server will try to detect the projections that are used and inject the projectionDefs automatically. The projectionDefs are stored in this
file: https://github.com/oskariorg/oskari-server/blob/master/control-base/src/main/resources/fi/nls/oskari/control/view/modifier/bundle/epsg_proj4_formats.json.
If the projection you want to use isn't available in the file you can insert the definition manually to the database config or make a pull request/issue to oskari-server
 to add the missing projection to the epsg_proj4_formats.json file.

The easiest way to change only the projection is to query the existing config from the database, modify the above projection specific configs and update the
 modified version back to the database.
The projection can be configured to the database with the following kind of SQL.
**Change the configuration and the view_id to match your application!**

```sql
    UPDATE portti_view_bundle_seq
    set config = '{ ... modified config ...}'
    WHERE
    bundle_id = (SELECT id FROM portti_bundle WHERE name = 'mapfull')
    AND view_id = [the view id you want to update]
```
