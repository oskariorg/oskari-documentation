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
- WFS layers provide a link to feature data (referred to as the Feature property data in the admin panel)
- WFS layer style can be changed on a temporary basis
- GeoServer backed WMS layers can be visualized as heatmaps
- For WMS layers the available styles can be viewed and selected for use

## Map Legends (Flyouts)

The map legend for all the selected layers can be opened from the menu and it opens as a flyout. The flyouts can be moved around the map window and closed by the user or programmatically. The flyout size adapts to screen size and some flyouts can be resized by the user.
