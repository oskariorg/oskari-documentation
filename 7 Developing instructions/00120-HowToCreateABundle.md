## How to create a bundle

If you haven't done it already, copy the oskari-frontend repo [here](https://github.com/oskariorg/oskari-frontend/).

Now decide a `<bundle-identifier>` which is unique and describes the functionality the bundle offers e.g. `search` (already implemented so prefix it with something like mysearch).

Create a folder with the name of your `<bundle-identifier>` under `/bundles/framework/`. If you require styling/images, create a folder under `/bundles/framework/<bundle-identifier>/resources/css`, too. The `/framework/` directory isn't enforced and you can replace it with something fitting your bundle compilation. The `framework` directory refers to the namespace of the same name and it includes (almost) all code written by the Oskari core team. 

You can create your own namespace (and folders) for your own bundles. If you plan to contribute the bundle to oskari-frontend, place the bundle either to framework, mapping or admin depending on what the functionality is/does.

Create a `index.js` file under `bundles/framework/<bundle-identifier>/`. You can use the sample file as a template (explained below).

### The Sample bundle

A file named `index.js` under `/bundles/<mynamespace>/<bundle-identifier>/` folder should contain this sort of content. It defines that the bundles implementation file instance.js is located under `/bundles/mynamespace/<bundle-identifier>/` and localization data under that in `resources/locale/<lang>.js` files. At the end it installs the bundle to the Oskari framework so it can be started by the Oskari loader. 

Change the `<bundle-identifier>` and `<mynamespace>` to the identifiers of your choice before actually using this sample template.

```javascript
import { MyBundleInstance } from './instance';

// Register a factory function for the bundle id '<bundle-identifier>'
Oskari.bundle('<bundle-identifier>', () => new MyBundleInstance());
```

### Editing the sample bundle

* Change all the `<bundle-identifier>`s
* Add the implementation for `MyBundleInstance` class in `instance.js` file (or filename to match the class)
* Change the bundle instance name (`MyBundleInstance`). Usually its the bundle name postfixed with "Instance".
* You can use `import` to any css/scss files on the index.js OR in the instance.js file
* Add any localization files under `resources/locale`.

Create a `instance.js` file under `/bundles/framework/<bundle-identifier>/`. The `instance.js` is a file that is referenced as `import` on `index.js` . You can use the `SampleInfoBundleInstance.js` file from https://github.com/oskariorg/sample-application/blob/2.0.0/bundles/sample-info/SampleInfoBundleInstance.js as a template.

The `instance.js` file is usually responsible for creating all the classes a bundle might need during its lifetime, registering possible request handlers and registering the bundle to the sandbox to be able to listen to events.

By extending `DefaultExtension` you can forget the nitty gritty details and focus on writing the application logic instead. All the functions can be overridden should you need to do something differently (in the example below, we override the `getName` function as it returns the name from config by default). Refer to the [API documentation](https://oskari.org/documentation/api) to see all the functions of `DefaultExtension`.

Add your bundle to the applications `main.js` (change the path and `<bundle-identifier>` to match where your index.js is located):

```javascript
import oskari-bundle!oskari-frontend/bundles/framework/<bundle-identifier>;
```

Start adding your code in `instance.js`. If you have lots of code it is encouraged to add multiple `.js` files beside the `instance.js` (under the bundles implementation folder structure). These files can define other Oskari classes that the `instance.js` creates and operates. 

See also [7.1.6 How to use a bundle](../7 Operating instructions/00080-HowToUseABundle.md).