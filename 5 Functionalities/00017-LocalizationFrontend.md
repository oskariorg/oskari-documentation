#### Localization in frontend

Localization files are provided by bundles in `resources/locale` folder under the bundle implementation (relative to the bundles `index.js` file). Using this convention allows the `oskari-bundle` and `oskari-lazy-bundle` Webpack loaders to optimize packaging the translations for a specific application to a single localization file per language and the files don't need to be referenced manually since the loaders search that folder automatically.

Note that older `bundle.js` based bundles can reference the localization files in bundle definitions (`bundle.js` - locales array).

Each languages localization is in its own file with structure like this:

```javascript
Oskari.registerLocalization({
    // localization language
    "lang" : "en",
    // Bundle id or some other identifier which is used when retrieving this localization data
    "key" : "<MyBundlesLocalizationKey>",
    "value" : {
        // Actual localization data in custom structure, bundle using this data is responsible for interpreting the structure
        "title" : "Bundle title",
        "steps" : {
            "step1" : {
                "tab" : "Step 1",
                "title" : "Step 1 title",
                "content" : "{name}'s layer was created on {created, date}"
            },
            "step2" : {
                "tab" : "Step 2",
                "title" : "Step 2 title",
                "content" : "Remove {count, plural, one {# map layer} other {# map layers}}"
            },
            "step3" : {
                "tab" : "Step 3",
                "title" : "Step 3 title",
                "content" : "No interpolation here"
            }
        }
    }
});
```

The message strings are in [ICU message syntax](https://formatjs.io/guides/message-syntax/). Value names to be included into the string are in curly braces. Type (number, date or time) of the value can be given after the comma (default is string). The number/date/time will be formatted according to the locale rules (see steps.step1.content above). If the language has different forms for singular/plural, all forms must be defined (see steps.step2.content above). Note: some languages have [multiple plural forms](https://formatjs.io/guides/message-syntax/#plural-format). It also possible to define [different messages depending on value](https://formatjs.io/guides/message-syntax/#select-format).

The language that will be returned is the one set with `Oskari.setLang()` function (usually done automatically before application start):

```javascript
Oskari.setLang('en');
```

##### Formatting

To get a localized string in the current language:

```javascript
Oskari.getMsg('<MyBundlesLocalizationKey>', '<path.to.message>', { key1: value1, key2: value2 });
```
Where keys are the ones used inside curly braces in the ICU format. From the example above:

```javascript
Oskari.getMsg('<MyBundlesLocalizationKey>', 'steps.step1.content', { name: 'Oscar', created: new Date() });
Oskari.getMsg('<MyBundlesLocalizationKey>', 'steps.step2.content', { count: 4 });
Oskari.getMsg('<MyBundlesLocalizationKey>', 'steps.step3.content');
```

To avoid typing the bundles's localization key in every localization call, you can bind the first argument and create an instance variable for it:

```javascript
Oskari.clazz.define(
    'Oskari.my.bundle.clazz',
    function () {
        this.loc = Oskari.getMsg.bind(null, '<MyBundlesLocalizationKey>');
    }, {
        someMethod: function() {
            this.loc('steps.step1.content', {name: 'Oscar', created: new Date()});
        }
    },
    ...
```

#### Overriding localization in application

Existing localization can be overridden in applications so they make more sense in the application context. An example of this could be overriding the info text for what the end-user can expect the search functionality to give as results.
 Usual way of doing this is making an application specific bundle with an id like `lang-overrides` (the id can be anything really) that includes localization files with overriding values for existing translations and import it in the application.

```javascript
Oskari.registerLocalization({
    // localization language
    "lang" : "en",
    // An existing bundle localization key you want to override
    "key" : "<MyBundlesLocalizationKey>",
    "value" : {
        // Keys that you want to override, this can be a subset and only the provided ones are overridden
        "steps" : {
            "step1" : {
                "tab" : "Step 1 (Customized)",
                "content" : "The glorious map layer of user '{name}' was created on {created, date}"
            }
        }
    }
}, true);
```
The notable part is the **second parameter** for `Oskari.registerLocalization()` with `true` as the value will merge the object that is provided as the first parameter with an existing localization with the same localization key, overriding any localization values that are provided and using the existing ones where missing. If the parameter is missing or `false` (usually missing for regular localization files) the localization object is also merged to an existing one, but not overriding existing values. So it doesn't matter if the lang overrides are added before bundles that have the original texts or after them, as long as the second parameter for the overrides is `true`.

#### Using localization string in React component

Localizations can be provided with React context by importing Oskari `LocaleProvider` and having the React component tree as it's child elements like this:

```javascript
import { LocaleProvider } from 'oskari-ui/util';
```

```xml
<LocaleProvider value={{ bundleKey: 'MyBundlesLocalizationKey' }}>
    ...
    <Message messageKey={'steps.step1.tab'}/>
    <Message messageKey={'steps.step1.content'} messageArgs={{ name: 'Oscar', created: new Date() }}/>
    ...
</LocaleProvider>
```

##### Deprecated old way

In some even older bundles the whole localization data tree is retrieved on runtime with a call:

```javascript
var localization = Oskari.getLocalization('<MyBundlesLocalizationKey>');
this.showMessage(localization.steps.step1.tab);
```

This way does not support interpolation, pluralization or number/time formatting and therefore should not be used in new bundles.
