### How to use publisher tools

Bundles in Oskari can provide a tool object(/Oskari clazz) that the `publisher` bundle/functionality discovers at runtime. This allows bundles to extend the publisher user-interface by adding new options for the user to see when that particular bundle is part of an application.

The tools are discovered by querying Oskari for classes with protocol `Oskari.mapframework.publisher.Tool`
 (See example tool class below with `'protocol': ['Oskari.mapframework.publisher.Tool']`).

Starting with Oskari 3.0 all tool panels and publisher tools provided by Oskari are written in es / react. Using jQuery for creating custom publisher tools (even though old jquery-implementations might still work) is highly discouraged.

With ES class syntax:

```javascript
const AbstractPublisherTool = Oskari.clazz.get('Oskari.publisher.AbstractPublisherTool');
const BUNDLE_ID = 'mybundle';
class MyPluginTool extends AbstractPublisherTool {
    getTool () {
        return {
            id: 'Oskari.mybundle.MyPlugin',
            // title is the UI label for the tool
            title: Oskari.getMsg(BUNDLE_ID, 'tool.label'),
            config: this.state.pluginConfig || {}
        };
    }
    // process the embedded map "data"/app setup and detect if this tool was enabled on it
    init (data) {
        if (!data || !data.configuration[BUNDLE_ID]) {
            return;
        }
        const conf = data.configuration[BUNDLE_ID].conf || {};
        this.storePluginConf(conf);
        this.setEnabled(true);
    }
    // return values to be saved based on if this tool was enabled or not
    getValues () {
        if (!this.isEnabled()) {
            return null;
        }
        return {
            [BUNDLE_ID]: {
                whatever: 'config'
            }
        }
    }
}

// Attach protocol to make this discoverable by Oskari publisher
Oskari.clazz.defineES('Oskari.publisher.MyPluginTool',
    MyPluginTool,
    {
        'protocol': ['Oskari.mapframework.publisher.Tool']
    }
);
```

When the user starts the publisher:
1) `UIChangeEvent` is sent to notify other functionalities to shut themselves down to avoid conflicts for screen space.
2) `StateHandler.SetStateRequest` is used to set the application state (based on the embedded map that is being edited).
3) `mapmodule` is queried for plugins with `plugin.isShouldStopForPublisher()` returning true (this defaults to `plugin.hasUI()` returning true). All of these plugins are shutdown/stopped to clean the map state for publisher functionality.
4) Gathers/discovers all tool classes that are available on the application, creates an instance of them and groups the tools based on the group value of the tools
5) Creates the panels and calls `tool.init(data)` for all tools
6) Any panels that have tools where `tool.isDisplayed(data)` returns true are shown to the end user

When the user exits the publisher:
1) On save: calls `tool.getValues()` to gather a payload for saving the embedded map based on end user selections to database
2) calls `tool.stop()` - this is where the tool should clean up anything that it has started when the publisher functionality was running
3) Starts all plugins that were stopped on the step 3 of startup to restore normal geoportal functionality.

#### Publisher tool API

Many of these are handled by the `publisher/tools/AbstractPublisherTool` base class. Take a look at it before overriding the functions:

- `init(dataObject)` receives the data for the embedded map and should use that data to detect if the tool is enabled when a user edits an embedded map.
- `getTool()` This should return an object with keys:
    - id: the value should match a plugin class name that this tool controls. If this doesn't control a plugin it can be any unique value, but then you need to override some of the functions in base class
    - title: This should return a label that is shown to the user as a selection what this tool does (jQuery-based tools assume this is a key reference to get the label from localization)
    - config: This should return the plugin config that the tool.init() received when the tool is started
    - hasNoPlugin: If the tool is NOT handling a plugin that is referenced in the id, you can return true here to have `AbstractPublisherTool` skip starting and stopping the plugin.
    - disabledReason: Oskari.getMsg('mybundle', 'tool.toolDisabledReason') This can be used to return a string to show the user as a tooltip when the tool is disabled for letting the user know why it can't be selected.
- `getComponent()` This is used by tools that have extra options to render. Returns an object that can have keys (if keys are missing the tool doesn't have extra options):
    - component: React-component that is rendered to show the extra options for this tool
    - handler: Reference to a `UIhandler` for handling the extra options
- `setEnabled(boolean)` Most tools should just use the version from AbstractPublisherTool. Override only when necessary. Even then it's probably safest to add a call to super() in your overridden function and do your custom thing only afterwards. Starts/stop the functionality on the map (like map plugin). Should send `Publisher2.ToolEnabledChangedEvent` to allow the plugin placement mode to be activated when a tool is selected when the mode is active. Called when the user selects/deselects the tool on the publisher.
- `isEnabled()` should return true if the tool is selected
- `getValues()` should return how the user selections for the tool should affect the appsetup JSON on the embedded map. This should match the logic that `init()` uses for detecting if the tool should be enabled when editing an embedded map.
- `stop()` called when the user exits publisher functionality. The tool should clean up after itself on this function (for example shutting down things added to the map while on publisher).
- `isDisplayed()` should return true if a selection for the tool should be shown to user. The function receives the publisher data as parameter and can use the data to detect if it should be shown. Most of the tools return true, but some examples of cases for this are:
    - the feature data table isn't shown if there are no layers that have feature data
    - LogoPlugin isn't displayed as a selection to user as it's always included. However a tool is provided for it to allow user to select the placement for the logo on the embedded map.
    - statistical data tools return false if there are no statistical data on the map
- `isDisabled()` related to isDisplayed(). This allows the selection to be shown even if user can't select it. A tooltip can be included to show the user why the tool is disabled. An example is that map legends selection is shown but is disabled if layers on the map don't have legends available.

#### Tool, component and handler

When a tool has more options than just enabling or disabling a plugin on map we need to provide a component for rendering extra options in publisher as well as a handler to maintain the component's state and possibly handle more complex logic. Here's an example how one would go forth to add a new react tool to map tools panel.

##### The tool

First we would need to create the actual tool class. To keep things simple this will just control an existing plugin (index map) on the map and have some bogus extra options. The tool class needs to implement the `Oskari.mapframework.publisher.Tool` - protocol to be discoverable by the publisher. Also the code of the tool class needs to be imported somehow. For tools that handle mapmodule's plugins there is the file `bundles/mapping/mapmodule/publisher/tools.js` which imports / exports all those tools.

```javascript
import { AbstractPublisherTool } from '../../../../framework/publisher2/tools/AbstractPublisherTool';
import { IndexMapToolComponent } from './IndexMapToolComponent';
import { IndexMapToolHandler } from './IndexMapToolHandler';

class IndexMapTool2 extends AbstractPublisherTool {
    constructor (...args) {
        super(...args);
        // index of the tool within the panel
        this.index = 30;
        // id of the panel this tool belongs to
        this.group = 'tools';
        this.handler = new IndexMapToolHandler(this);
    }

    init (data) {
        super.init(data);
        const config = this.state?.pluginConfig || {};
        // restore config for saved map
        this.handler.init(config);
    }

    getTool () {
        return {
            id: 'Oskari.mapframework.bundle.mapmodule.plugin.IndexMapPlugin',
            title: Oskari.getMsg('MapModule', 'publisherTools.IndexMapPlugin.toolLabel') + ' 2',
            config: this.state.pluginConfig || {}
        };
    }

    getComponent () {
        // returns the component to render extra options in publisher and the statehandler for this tool
        return {
            handler: this.handler,
            component: IndexMapToolComponent
        };
    }

    getValues () {
        if (!this.isEnabled()) {
            return null;
        }

        const pluginConfig = this.getPlugin().getConfig() || {};
        // handler's state contains the extra options stacked on top of AbstractPublisherTool's defaults
        const state = this.handler.getState();
        for (const key in state) {
            if (state.hasOwnProperty(key)) {
                pluginConfig[key] = state[key];
            }
        }

        return {
            configuration: {
                mapfull: {
                    conf: {
                        plugins: [{ id: this.getTool().id, config: pluginConfig }]
                    }
                }
            }
        };
    }
}

// Attach protocol to make this discoverable by Oskari publisher
Oskari.clazz.defineES('Oskari.publisher.IndexMapTool2',
    IndexMapTool2,
    {
        protocol: ['Oskari.mapframework.publisher.Tool']
    }
);

export { IndexMapTool2 };
```

##### The component

The component is basically any react-component containing the extraoptions (if any) shown when the tool is enabled. In this case it's just a checkbox that is either on or off.

```javascript
import React from 'react';
import { Message, Checkbox } from 'oskari-ui';

export const IndexMapToolComponent = ({ state, controller }) => {
    const { myExtraChoiceForIndexMap } = state;

    return <>
        <Checkbox checked={myExtraChoiceForIndexMap} onChange={evt => controller.myExtraChoiceForIndexMapChanged(evt.target.checked)}>
            <Message bundleKey={'MapModule'} messageKey={'publisherTools.IndexMapPlugin.myExtraChoice'}/>
        </Checkbox>
    </>;
};
```

##### The handler

The handler handles the tool's and component's state and could be used to handle more complex logic as well. Often the state can be used as-is when saving and restoring the published map.

```javascript
import { StateHandler, controllerMixin } from 'oskari-ui/util';

class UIHandler extends StateHandler {
    constructor (tool) {
        super();
        this.tool = tool;
        // set the extra choice off by default
        this.setState({
            myExtraChoiceForIndexMap: false
        });
    };

    init (pluginConfig) {
        // this will restore whatever the state of the extra choice was for the published map
        this.updateState({
            ...pluginConfig
        });
    }

    myExtraChoiceForIndexMapChanged (checked) {
        const newState = {
            myExtraChoiceForIndexMap: checked
        };

        this.updateState(newState);
    }
}

const wrapped = controllerMixin(UIHandler, [
    'myExtraChoiceForIndexMapChanged'
]);

export { wrapped as IndexMapToolHandler };
```