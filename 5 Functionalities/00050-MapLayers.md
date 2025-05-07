## Map layers

Oskari supports various APIs for map layers: WMS, WMTS, WFS, ArcGIS Cache layers and ArcGIS REST feature layers. The map layers can be added by the admin - the instructions for this can be found from the Admin FAQ.

## Map layer listing

The map layers can be accessed via a map layer selector. The Map layer selector contains a list of all available map layers that the user has permission to see in the Oskari instance. The layer list can be grouped either by theme or by data provider and filtered by user input or preconfigured filters like based on layer type or just showing the latest additions to layers. The map layer selector can be used to toggle map layers on the map, filter them and to access their metadata. To view metadata there is link to the metadata display module, which accesses a configured CSW service.

Supported layer types include:
- map layer
- map layer stack
- vector feature layers
- region set for statistical data
- user's own data layer
- time-enabled WMS layer (WMS-T)

It is possible to add new map layer types and while defining a new type of layer you can also configure what fields are relevant for that type for the map layer admin functionality.

Layers can have:
- permissions on who can see them
- localized names (based on supported languages configuration)
- link to metadata (prepopulated by service capabilities)
- link to a legend image (prepopulated by service capabilities)
- vector layers can have styles and formatting for attribute data

### Selected map layers window

Oskari UI lists the layers that are on the map in a separate tab of the map layer listing as selected map layers.

The functionalities of Selected map layers window are:

- Layers can be temporarily hidden and shown
- Layers can be organized in the layer stack by dragging and dropping
- Layer opacity can be controlled using a slidebar or by entering opacity percentage
- Layer metadata can be viewed (fetched for display using CSW interface)
- WFS layers provide a link to feature data (referred to as the Feature property data in the admin panel)
- GeoServer backed WMS layers can be visualized as heatmaps
- For WMS layers the available styles can be viewed and selected for use
- For WFS layers users can define their own styles in addition to using ones added by admins

Any functionalities can hook into the layers and programmatically add "tools" to them which can be shown to the end user per layer on the selected layers listing. Some examples are:
- WFS layers provide a link to feature attribute data table
- GeoServer backed WMS layers can be visualized as heatmaps

## Map Legends (Flyouts)

The map legend for all the selected layers can be opened from the menu and it opens as a flyout. The flyouts can be moved around the map window and closed by the user or programmatically. The flyout size adapts to screen size and some flyouts can be resized by the user.
