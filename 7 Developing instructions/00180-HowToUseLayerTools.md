## How to use layer tools

A layer can have two types of tools. Layer tools which target the layer as a whole, and feature tools that target individual features within the layer.

Tools are shown for example in the layer list next to the layer entry, while feature tools appear per feature row in the feature data table (edit, remove) and in the get info popup. Both types are instances of `Oskari.mapframework.domain.Tool`.

### Map Layer API for handling tools

For layer tools:

- `getTools()`
- `setTools(tools)`
- `addTool(tool)`
- `getTool(toolName)`

For feature tools:

- `getFeatureTools()`
- `setFeatureTools(featureTools)`
- `addFeatureTool(featureTool)`
- `getFeatureTool(toolName)`

### Add a layer tool

```javascript
const mapLayerService = sandbox.getService('Oskari.mapframework.service.MapLayerService');
const layer = mapLayerService.findMapLayer(layerId);

const tool = Oskari.clazz.create('Oskari.mapframework.domain.Tool');
tool.setName('my-layer-tool');
tool.setTitle('My layer action');
tool.setTooltip('Do something for this layer');
tool.setCallback(() => {
	sandbox.postRequestByName('MyBundle.MyRequest', [layer.getId()]);
});

mapLayerService.addToolForLayer(layer, tool);
```

You can add a tool for layer either via `mapLayerService.addToolForLayer(layer, tool)` or to the layer directly by `layer.addTool(tool)`. The former call notifies listeners
with a `MapLayerEvent` with operation `tool`.

### Add a feature tool

Feature tools are added directly to layer.

```javascript
const featureTool = Oskari.clazz.create('Oskari.mapframework.domain.Tool');
featureTool.setName('edit-feature');
featureTool.setTitle('Edit feature');
featureTool.setIconComponent(<EditOutlined />);
featureTool.setTypes([]);
featureTool.setCallback((layerId, featureId) => {
	sandbox.postRequestByName('ShowFeatureEditorRequest', [layerId, featureId]);
});

layer.addFeatureTool(featureTool);
```

The callback signature for feature tools is `(layerId, featureId)`.
