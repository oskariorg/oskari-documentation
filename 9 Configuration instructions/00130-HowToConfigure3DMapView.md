## How to configure 3D map view

Oskari supports 3D map view since version 1.54 as an optional 3D-capable mapmodule has been added to Oskari. 3D mapmodule is based on [Ol-Cesium library](https://openlayers.org/ol-cesium/), which is meant for integrating OpenLayers and Cesium. Since Oskari 2D uses OpenLayers for map rendering and many map tools are built using OpenLayers most of the tools work out-of-the-box in 3D with Ol-Cesium.

Oskari supports 3D Tiles for 3D data rendering. [3D Tiles](https://www.opengeospatial.org/standards/3DTiles) is an OGC Community standard and is designed for streaming and rendering massive 3D geospatial content.

Example 3D application can be seen on [demo.oskari.org](https://demo.oskari.org/3d).

### 3D map view setup frontend

Oskari [Sample application](https://github.com/oskariorg/sample-application) has example 3D application (geoportal-3d) and publishing template for embedded 3D maps (embedded-3d).

The changes from a usual 2D geoportal are listed below:

#### Add Ol-Cesium packages to main.js

Mandatory requirements is to add Ol-Cesium based mapmodule and layerplugin for 3D Tiles.

```
// Remove the default mapmodule import if you have one ('oskari-loader!oskari-frontend/packages/mapping/ol/mapmodule/bundle.js')

import 'oskari-loader!oskari-frontend/packages/mapping/olcs/mapmodule/bundle.js';
import 'oskari-loader!oskari-frontend/packages/mapping/olcs/map3dtiles/bundle.js';

```

Oskari frontend contains additional bundles for controlling camera and displayed time.
```
import 'oskari-loader!oskari-frontend/packages/mapping/time-control-3d/bundle.js';
import 'oskari-loader!oskari-frontend/packages/mapping/camera-controls-3d/bundle.js';
```
Cesium adjusts the sun's position and visualizes the shadows according to the time set for the map. You can also adjust displayed time with [SetTimeRequest](https://oskari.org/documentation/api/requests/latest/SetTimeRequest).

#### Using 2D and 3D maps side by side

You can have different appsetups for 2D and 3D maps like in https://demo.oskari.org/. Oskari frontend contains bundle for changing appsetups which adds 2D or 3D button to toolbar for switchint between the appsetups.
```
import 'oskari-loader!oskari-frontend/packages/mapping/dimension-change/bundle.js';
```
*Note!* You need to import 3D tiles layerplugin also with 2D geoportal application to support layer listing.

### D map view setup backend

Oskari [Sample server-extension](https://github.com/oskariorg/sample-server-extension) has example Flyway module [example3d](https://github.com/oskariorg/sample-server-extension/tree/master/app-resources/src/main/java/flyway/example3d) to set backend ready for 3D. Oskari Jetty bundle's sample [configuration](https://github.com/oskariorg/sample-configs/blob/master/jetty-9/oskari-server/resources/oskari-ext.properties) doesn't have 3D enabled by default. You should add `example3d` to `db.additional.modules`.

Example view configuration [geoportal-3d.json](https://github.com/oskariorg/sample-server-extension/blob/master/app-resources/src/main/resources/json/views/geoportal-3d.json).

#### Add 3D tiles plugin to mapfull bundle's plugins array

`{ "id": "Oskari.mapframework.mapmodule.Tiles3DLayerPlugin" }`

#### Setting camera and time state

Optional camera settings and displayed time can be given in `mapfull's` bundle state.

#### Configuring the terrain

Optional terrain configuration can be given in `mapfull's` bundle configuration under `mapOptions`.

```javascript
"mapOptions": {
    "terrain": {
        "providerUrl": "<url>",
        "ionAssetId": "<asset-id>",
        "ionAccessToken": "<your-ion-access-token>"
    }
}
```
`terrain` and all it's children are optional. 
 - Using `providerUrl` one can set the TMS service end point. Supported terrain formats are heightmap-1.0 and quantized-mesh-1.0. 
 - When `ionAssetId` is provided, the terrain is streamed from Cesium ion service which also requires `ionAccessToken`. 
 - When `ionAccessToken` is provided without `ionAssetId` Cesium World Terrain is used for providing detailed terrain all over the globe.

Configuration can be set also in oskari-ext.properties which overrides `mapfull`'s terrain configuration:

    oskari.map.terrain.url=<url>
    oskari.map.terrain.token=<your-ion-access-token>
    oskari.map.terrain.asset=<asset-id>

#### Configuring dimension change

Dimension change bundle loads page with different appsetup. Changing between 2D and 3D appsetups bundle should have wanted view's uuid in the configuration.
`{"uuid": uuidToLoad}`

#### Enable 3D map publishing

Embedded 3D maps needs own template which have to be set to 3D view. The uuid of the appsetup for 3D-publish template needs to be configured to the 3D geoportal appsetup [metadata](https://github.com/oskariorg/sample-server-extension/blob/1.3.0/app-resources/src/main/java/flyway/example3d/V1_1__setup_3D_publishing.java).

### Adding your own 3D data

3D tilesets can be added to the map using admin tools and choosing 3D Tiles as the layer type. Example application http://demo.oskari.org has 3D model and buildings with textures layers from City of Helsinki.

### Configuring 3D-tiles map layers

The 3D-tiles maplayers are saved in the database with all other datasets/services used for map layers in Oskari.
The default fill color for 3D objects like buildings is a brownish tint/color in Oskari. This might make a textured surface look "dirty" by default but you can configure this. Also a tileset including a mesh-type scanned 3D environment has pretty low resolution with default settings in Cesium JS. Both of these can be tuned with database configuration in oskari_maplayer table by configuring the options column:

```
UPDATE oskari_maplayer
SET options='{
    "maximumScreenSpaceError": 0,
    "styles": {
        "buildings": {
            "fill": {
                "color":"#FFFFFF"
            }
        }
    }
}'
 where id = [layer id for 3d tileset to configure];
```

The maximumScreenSpaceError defaults at value 16 in Cesium and affects the mesh type tileset resolution heavily.
The styles value is similar to oskari style JSON but the styling isn't fully supported yet for 3D-tiles. The `buildings` part of the snippet above is the style name and you can choose it yourself. To make textured objects look better you might want to set the fill color to white.

### Oskari 3D current state

The released version of 3D map is the initial version which can be developed further based on requirements that will emerge from use cases. The following features have been developed to support the use of 3D map:

* adding 3D Tiles datasets
* 3D map publishing
* support for WFS map layers and feature data in 3D map view
* camera flights with RPC API
* measure tools, coordinate tool, markers and own places extended to work also in 3D map view
* support for adding tilesets from Cesium ion platform with admin tool

### Creating your own 3D datasets using Cesium Ion

[Cesium ion](https://cesium.com/cesium-ion/) is a platform for 3D geospatial data tiling, hosting and streaming. It converts different types of data (e.g. point clouds, 3D buildings, photogrammetry) to 3D Tiles format. A free Community account can be used for non-commercial personal projects, exploratory development, or unfunded educational activities within the defined usage limits. See the pricing page for more information.

Run in console
```
var mapmodule = Oskari.getSandbox().getStatefulComponents().mapfull.mapmodule;
mapmodule.getCesiumScene().primitives.add(
    new Cesium.Cesium3DTileset({
        url: Cesium.IonResource.fromAssetId(yourAssetId, {
            accessToken: 'your-access-token-here'
        })
    })
);
```
