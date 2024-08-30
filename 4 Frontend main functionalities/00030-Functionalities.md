# Frontend main functionalities

In the next subsections some of the core functionalities of the Map window, Map layer selector and Menu in Oskari are listed and explained in more detail.

## Map window functionalities

The map window offers various functionalities, some of which are controlled with mouse, others as separate tools. Some of the functions can also be configured programatically.

The following are **mouse-click functionalities** for the end-user:
- Panning map view
- Zoom in by double clicking on the map
- GetFeatureInfo (GFI) action with a single click on all map layers with GFI enabled, including WFS layers

The map window has **separate tools** to perform the following:
- Restore the initial map state
- A scale bar that updates with the map scale
- Coordinate display that adapts according to configured coordinate system

Map window has functions that can **configured programatically**:
- Coordinate system
- Zoom bar (appearance and number of zoom levels)
- Tile size and image format parameters

## Map layers

Oskari supports various APIs for map layers: WMS, WMTS, WFS, ArcGIS Cache layers and ArcGIS REST feature layers. The map layers can be added by the admin - the instructions for this can be found from the Admin FAQ.

## Map layer selector

The map layers can be accessed via map layer selector. The Map layer selector contains a list of all available map layers in the Oskari instance, grouped either by theme or by data provider. The map layer selector can be used to show map layers, filter them and to access their metadata. To view metadata there is link to the metadata display module, which accesses CSW interface.

Supported layer types are:
- map layer
- map layer stack
- background map layer
- WFS layer
- thematic map layer
- analysis results layer
- user's own data layer
- time-enabled WMS layer (WMS-T)

If needed, new map layer types can be also added programmatically.

## Selected map layers window

Oskari UI has a separate window for the selected map layers. Here are listed all the layers displayed in the map view.

The functionalities of Selected map layers window are:

- Layers have localized titles (and optionally subtitles)
- Layers can be temporarily hidden and shown
- Layers can be organized in the layer stack by dragging and dropping
- Layer opacity can be controlled using a slidebar or by entering opacity percentage
- Layer metadata can be viewed (fetched for display using CSW interface)
- WFS layers provide a link to attribute information table
- WFS layer style can be changed on a temporary basis
- GeoServer backed WMS layers can be visualized as heatmaps
- For WMS layers the available styles can be viewed and selected for use

## Menu and toolbar

The whole Oskari UI can be personalized by the main users. This includes the menu on the side of the map window. The menu can be, for example, hidden and the individual menu entries can be configured individually.

Toolbar is anchored to the menu bar and contains various tools. The toolbar can be hidden and different buttons can be disabled and enabled individually. Tools can also be made to appear when a specific map layer is added to selected layers list.

**The toolbar buttons include**:
- Drag & pan button (enabled by default)
- Rubber band zoom button
- Restore initial map state button
- Map view history manager: navigation with back and forward buttons
- Measurement tools: measure distance and area
- Link map view: creates an URL which opens the current map view
- Marker tool: markers with associated text can be created on the map and sent as link
- Print map view: starts the print mode
- Save map view: Map view configurations can be saved by logged-in user
- Find nearest place tool: Find the nearest place name by clicking on the map

## Location search and metadata search

Oskari has search functions for certain locations and the metadata of map layers.

**The location search** is a one-field search which can return search results from multiple sources, such as placename, address, cadastral parcel or similar services. The service access parameters are configurable in the backend.

**The metadata search** retrieves metadata from a CSW backend and offers advanced search options. Users can search for keywords within specific resource types, metadata languages, or layers published by selected organizations. After a successful search, the resulting layers can be viewed as a list or displayed on the map window.
