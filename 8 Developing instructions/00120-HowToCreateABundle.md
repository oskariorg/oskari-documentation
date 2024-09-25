## How to create a bundle

If you haven't done it already, download sources [here](). Then extract the files from the downloaded archive.

Now decide a `<bundle-identifier>` which is unique and describes the functionality the bundle offers e.g. 'search' (already implemented so prefix it with something like mysearch).

Create a folder with the name of your `<bundle-identifier>` under `/packages/framework/` and `/bundles/framework/`. If you require styling/images, create a folder under `/bundles/framework/<bundle-identifier>/resources/css`, too. The `/framework/` directory isn't enforced and you can replace it with something fitting your bundle compilation. The `framework` directory refers to the namespace of the same name and it includes (almost) all code written by the Oskari core team. It is encouraged to create your own namespace (and directories) for your own bundles.

Create a `bundle.js` file under `/packages/framework/<bundle-identifier>/`. You can use the following sample as a template.

### The Sample bundle

A file named `bundle.js` under `/packages/<mynamespace>/<bundle-identifier>/` folder should contain this sort of content. It defines that the bundles implementation file instance.js is located under `/bundles/mynamespace/<bundle-identifier>/` and localization data under that in `resources/locale/<lang>.js` files. At the end it installs the bundle to the Oskari framework so it can be started by the Oskari loader. 

Change the `<bundle-identifier>` and `<mynamespace>` to the identifiers of your choice before actually using this sample template.


    /**
    * Definition for bundle. See source for details.
     *
     * @class Oskari.<mynamespace>.<bundle-identifier>.MyBundle
     */
    Oskari.clazz.define("Oskari.<mynamespace>.<bundle-identifier>.MyBundle",
    
    /**
     * Called automatically on construction. At this stage bundle sources  
     have been
     * loaded, if bundle is loaded dynamically.
     *
     * @contructor
     * @static
     */
    function() {
    
    }, {
    /*
     * called when a bundle instance will be created
     *
     * @method create
     */
    "create" : function() {
        return Oskari.clazz.create("Oskari.<mynamespace>.<bundle-identifier>.MyBundleInstance");
    },
    /**
     * Called by Bundle Manager to provide state information to
     *
     * @method update
     * bundle
     */
    "update" : function(manager, bundle, bi, info) {
    }
    },

    /**
     * metadata
    */
    {
       "protocol" : ["Oskari.bundle.Bundle"],
        "source" : {
            "scripts" : [{
                "type" : "text/javascript",
                "src" : "../../../bundles/<mynamespace>/<bundle-identifier>/instance.js"
            }],
            "locales" : [{
                "lang" : "fi",
                "type" : "text/javascript",
                "src" : "../../../bundles/<mynamespace>/<bundle-identifier>/resources/locale/fi.js"
            }, {
                "lang" : "sv",
                "type" : "text/javascript",
                "src" : "../../../bundles/<mynamespace>/<bundle-identifier>/resources/locale/sv.js"
            }, {
                "lang" : "en",
                "type" : "text/javascript",
                "src" : "../../../bundles/<mynamespace>/<bundle-identifier>/resources/locale/en.js"
                }]
        }
    });

    // Install this bundle by instantating the Bundle Class
    Oskari.bundle_manager.installBundleClass("<bundle-identifier>", "Oskari.<mynamespace>.<bundle-identifier>.MyBundle");

### Editing the sample bundle

* Change all the `<bundle-identifier>`s
* Change the bundle name (MyBundle) to something more describing.
* Change the bundle instance name (MyBundleInstance). Usually its the bundle name postfixed with "Instance".
* List all the implementation files and css files under `scripts`. Note that CSS files need to have `"type" : "text/css"`
* List all the localization files under `locales` or you can remove the locales property if your bundle does not include localization.

Create a `instance.js` file under `/bundles/framework/<bundle-identifier>/`. `instance.js` is a file you referenced in `bundle.js` scripts. You can use the `instance.js` file from `oskari-frontend/bundles/sample/myfirstbundle/` as a template.

The `instance.js` file is usually responsible for creating all the classes a bundle might need during its lifetime, registering possible request handlers and registering the bundle to the sandbox to be able to listen to events.

By extending `DefaultExtension` you can forget the nitty gritty details and focus on writing the application logic instead. All the functions can be overridden should you need to do something differently (in the example below, we override the `getName` function as it returns the name from config by default). Refer to the [API documentation](.../api) to see all the functions of `DefaultExtension`.

Next, 
* Change all the `<bundle-identifier>`s
* Change the bundle instance name (MyBundleInstance). Use the same one you used before.

Add your bundle to the applications startup sequence like you did in previous steps (change the `<bundle-identifier>`):

```javascript
    {
        "bundlename" : "<bundle-identifier>",
        "metadata" : {
            "Import-Bundle" : {
                "<bundle-identifier>" : {
                    "bundlePath" : "../../../packages/framework/"
                }
            }
        }
    }
```

Start adding your code in `instance.js`. If you have lots of code it is encouraged to add multiple `.js` files beside the `instance.js` (under the bundles implementation folder structure). These files can define other Oskari classes that the `instance.js` creates and operates. You'll need to add references to these additional files in `bundle.js` scripts array.

See also [7.1.6 How to use a bundle](../7 Operating instructions/00080-HowToUseABundle.md).