# Other functionalities

Other important Oskari functionalities are listed here. Most of these require also the backend to work.

## Authentication and user management (requires backend)

Oskari has bundles for user management and for management of layers' priviledges. More information about the bundles will be added to this documentation later.

## Create data using My Data (requires backend)

The end-user of Oskari instance can create their own spatial data in the main map window. For this purpose, Oskari supports vector data, which means that points, lines and areas can be created and saved. Multigeometries and creation of holes is also supported.

The users can:
- Create multiple layers
- Configure the symbology for each layer separately
- Edit the description of map view and add links to external resources

## Import data (requires backend)

Users can import their own datasets to Oskari as zipped files. Supported formats are:

- Shapefile
- Mapinfo MID/MIF
- GPX trace
- KMX (zipped KML)

## Embedded maps with Create map mode (requires backend)

Create map mode is a tool for creating embedded map windows. The embeddable map window is shown in WYSIWYG mode. **The embedded map has same basic functions as the main map.**

Many of the tools available in the main map window are also available in the embedded map: scale bar, index map, zoom bar and search field. There are also map functions which can be enabled or disabled: panning by dragging, GetFeatureInfo. The size of the embedded map can also be configured. The created embedded maps are saved and can be later edited.

The embedded map parameters are name, website and language. The status of an embedded map can be either published or unpublished.

**Embedded map features**

- Zooming and panning
- Measure tools
- GetFeatureInfo
- WFS tabular data display
- Map layer menu
- Supported layer types
- WMS
- WMTS
- WFS
- My Data
- Imported data
- Thematic Maps and Tables
- ArcGIS rest Feature Layer
- Address, placename and real estate search
- Customizable layout (colours and tool positioning)
- Customizable size (preset size, fill space available)
- Find my location
- RPC API for interaction with the web site where the map is embedded
- RPC API features are listed separately here: http://oskari.org/examples/rpc-api/

## GetFeatureInfo

The GetFeatureInfo functionality is always active and starts by a single click / tap on the map. All GFI enabled map layers are queried on the single click. The response is shown in a popup dialog and can be styled using XSLT transformation (requires backend). Multiple response types are supported.

## Map Legends (Flyouts)

The map legend for all the selected layers can be opened from the menu and it opens as a flyout. The flyouts can be moved around the map window and closed by the user or programmatically. The flyout size adapts to screen size and some flyouts can be resized by the user.

## Print mode (requires backend)

The users of Oskari can print out what they have in the map window.

The print mode functionalities include:
- Automatic Print preview window
- Paper size and layout selection
- Printout format selection: PNG or PDF, also PDF/A
- Title, scalebar, date and a logo can be added, also text and markers of user's choice

## Thematic Maps (requires backend)

The GetFeatureInfo functionality is always active and starts by a single click / tap on the map. All GFI enabled map layers are queried on the single click. The response is shown in a popup dialog and can be styled using XSLT transformation (requires backend). Multiple response types are supported.

## WFS layers and attribute data table functionalities (requires backend)

- Complex schema WFS layers can be displayed
- Attribute data from WFS layers can be displayed in an attribute table
- Table view is synchronized with the map view: attribute rows are shown only for the features visible on the map
- Columns to be shown can be selected
- Data can be sorted ascending/descending
- Features can be highlighted by clicking either on the map or in the table rows
- Shift / Ctrl can be used to select multiple rows
