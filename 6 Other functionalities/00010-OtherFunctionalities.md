# Other functionalities

## More complex functionalities

Some of the most important but more complex Oskari functionalities are listed here. Most of these require also the backend to work.

### WFS layers and attribute data table

- Attribute data from WFS layers can be displayed in an attribute table
- Also complex schema WFS layers can be displayed
- Table view is synchronized with the map view: attribute rows are shown only for the features visible on the map
- Columns to be shown can be selected
- Data can be sorted ascending/descending
- Features can be highlighted by clicking either on the map or in the table rows
- Shift / Ctrl can be used to select multiple rows

### Create map mode

- Tool for creating embedded map windows
- The embeddable map window is shown in WYSIWYG mode
- The embedded map has same basic functions as the main map
- Embedded map parameters are name, website and language
- Size can be configured
- Tools available in the embedded map: scale bar, index map, zoom bar, search field
- Map functions which can be enabled or disabled: panning by dragging, GetFeatureInfo
- Created embedded maps are saved and can be later edited
- Status of an embedded map can be either published or unpublished

### My Data

- Points, lines and areas can be created and saved
- Multigeometries and creation of holes is supported
- Multiple layers can be created
- Symbology is separately configurable for each layer
- Description of map view can be edited and may contain links to external resources

### Print mode

- Paper size and layout selection
- Automatic Print preview window
- Printout format selection: PNG or PDF, also PDF/A
- Title, scalebar, date and a logo can be added, also text and markers of your choice

### Import data

- Supported format for importing data as zipped files
- Shapefile
- Mapinfo MID/MIF
- GPX trace
- KMX (zipped KML)

### Embedded map features

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
- Analysis results
- ArcGIS rest Feature Layer
- Address, placename and real estate search
- Customizable layout (colours and tool positioning)
- Customizable size (preset size, fill space available)
- Find my location
- RPC API for interaction with the web site where the map is embedded
- RPC API features are listed separately here: http://oskari.org/examples/rpc-api/rpc_example.html

### Thematic Maps

- Creation of thematic maps by joining statistical data and administrative units
- Importing own statistical indicators
- Downloading of indicator data into CSV/Excel formats

### Spatial Analysis

Available analysis methods:
- Buffer
- Descriptive statistics
- Union
- Clip
- Geometric filter
- Analysis Layer Union
- Buffers and sectors
- Difference Computation
- Spatial join
- Draw your own feature to be used in analysis
- Clip a feature
- The analysis tool also provides functionality to use search channel results as input for analysis.

### Authentication and user management

- Bundle for user management
- Bundle for management of layers privileges

### Digiroad Feature selector

- Adds a grid to display features fetched via WFS or some other protocol supported by OpenLayers
- Users can view the features and edit their attributes
- Features get added to the grid when the user clicks on the map. Alt and Ctrl keys can be used as modifiers
- A proxy is needed in case features are fetched via WFS
