# Frontend main functionalities

In the next subsections some of the core functionalities in Oskari are explained in more detail.

## Map window

- Supported layer types:
- WMS
- WMTS
- WFS (requires backend)
- ArcGIS Cache layers
- ArcGIS REST feature layers
- Tile size and image format parameters are configurable
- Coordinate system is configurable
- Coordinate display adapts according to configured coordinate system
- Zoom bar with a configurable number of zoom levels
- Pan map view
- Restore initial map state
- Scale bar

## GetFeatureInfo

- Always active, starts by a single click / tap on the map
- All GFI enabled map layers are queried on the single click
- Response is shown in a popup dialog
- Response can be styled using XSLT transformation (requires backend)
- Multiple response types are supported

## Mouse click functions

- Panning by dragging
- Zoom in by double clicking on the map
- GetFeatureInfo action with a single click on all map layers with GFI enabled
- Feature information is returned also from WFS layers

## Menu bar

- Menu entries can be configured individually
- Menu can be hidden (together with the toolbar)

## Location search

- One-field search which can return search results from multiple sources, such as placename, address, cadastral parcel or similar services
- Service access parameters are configurable in the backend

## Metadata search

- Search for metadata from a CSW backend
- Advanced search options
- Display resulting layers on the map

## Map layer selector

- List of available map layers, grouped either by theme or by data provider
- Add map layer to map view
- Filter layers function
- Show metadata for layers (link to the metadata display module, which accesses CSW interface)
- Supported layer types are:
- map layer
- map layer stack,
- background map layer
- WFS layer
- thematic map layer
- analysis results layer
- own data layer
- time-enabled WMS layer (WMS-T)
- New map layer types can be added programmatically

## Selected map layers

- List of layers displayed in the map view
- Layers have localized titles and subtitles (optional)
- Layer opacity can be controlled using a slidebar or by entering opacity percentage
- Layers can be temporarily hidden and shown
- Show metadata for layer (link to the metadata display portlet, which accesses CSW interface)
- Layers can be organized in the layer stack by dragging and dropping
- WFS layers provide a link to attribute information table
- WFS layer style can be changed on a temporary basis
- GeoServer-backed WMS-layers can be visualized as heatmaps
- For WMS-layers the available styles can be viewed and selected for use

## Map Legends

- Flyout shows map legends from all selected layers

## Toolbar

- Toolbar buttons can be disabled and enabled individually
- Toolbar can be hidden
- Restore initial map state button
- Map view history manager: back and forward buttons
- Rubber band zoom button
- Drag & pan button (enabled by default)
- Measurement tools: distance and area
- Marker tool: markers with associated text can be created on the map and sent as link
- Link map view: creates an url which opens the current map view
- Print map view: starts the print mode
- Save map view: Map view configurations can be saved by logged-in user
- Find nearest place tool: Find the nearest placename by clicking on the map
- Tools can be made to appear when a specific map layer is added to selected layers list

## Flyout manager

- Flyouts can be opened from the menu, moved around the map window and closed by the user or programmatically
- Flyout size adapts to screen size
- Some flyouts can be resized by the user
