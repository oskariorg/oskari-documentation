## Built-in Java annotations

Oskari provides custom annotations for Java classes that are used with `oskari-server`. The annotations allow a way for applications to hook into existing functionality without hard-coded references. For example applications can add handlers for `action routes` by just providing implementing classes on the classpath.

For enabling annotation processing (required for discovering annotated classes) on a Maven module you will need a file named `javax.annotation.processing.Processor` in the `src/main/resources/META-INF/services` folder of the module that has an annotated component. The content of the file references annotation processor(s) that should scan the module for any annotated classes like this:

```
# This Annotation processor will be run automatically by the compiler
# in order to detect all of the places that the @Oskari annotation
# is used.

fi.nls.oskari.annotation.OskariComponentAnnotationProcessor
```
You can also have several processors referenced in the same file separated on their own lines: 
```
fi.nls.oskari.annotation.OskariViewModifierAnnotationProcessor
fi.nls.oskari.annotation.OskariActionRouteAnnotationProcessor
```
The annotation processors are run at compile time and automatically generate a file that is included in the `jar`-file that references any annotated components in that module. The annotated files are then initialized using the [ServiceLoader](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/ServiceLoader.html) provided by Java. The annotated components can be discovered at runtime when they are referenced in that automatically generated file. 

### Oskari annotation

Requires the annotation processor `fi.nls.oskari.annotation.OskariComponentAnnotationProcessor`.

Java classes annotated with `@Oskari` need to extend the `fi.nls.oskari.service.OskariComponent` class and can be accessed with
`fi.nls.oskari.service.OskariComponentManager.getComponentOfType({class extending OskariComponent})`.

For example `OskariComponentManager.getComponentOfType(PermissionService.class)` returns an instance of `org.oskari.permissions.PermissionServiceMybatisImpl` because `PermissionServiceMybatisImpl` is extending `PermissionService` and is annotated with `@Oskari` annotation. This way you can get a reference to a concrete implementation class instance by querying with the service class without hard coding a reference to the implementation of a service.

There can be multiple implementations of the same base class with different "identifier" or "value".
 The value/id for the annotated class can be provided with `@Oskari("Somevalue")` and defaults to the name of the class if left undefined.

 For example any functionality that handles user related data can provide an implementation of the abstract `UserContentService` class.
 When a user is deleted, the data related to the user can then be removed by discovering annotated classes that handle user data.
 This allows the code that is responsible for a functionality to handle the removal of data related to the functionality
 without hard-coding references. The user deletion uses this pattern to find such data removal services:

```java
Map<String, UserContentService> userContentServices = OskariComponentManager.getComponentsOfType(UserContentService.class);
for (Map.Entry<String, UserContentService> entry : userContentServices.entrySet()) {
    // the identifier for annotated class (not used here, only documented)
    String serviceClass =  = entry.getKey();
    // call a method defined on UserContentService for deleting user related data
    // the implementing class just needs to be in the classpath and is not referenced directly
    entry.getValue().deleteUserContent(user);
}
```

### OskariActionRoute annotation

Requires the annotation processor `fi.nls.oskari.annotation.OskariActionRouteAnnotationProcessor`.

The annotion is used for adding handlers for Action routes (usually handlers of XHR requests made by the frontend).

Java classes annotated with `@OskariActionRoute` need to extend the `fi.nls.oskari.control.ActionHandler` class and can be accessed with
`fi.nls.oskari.control.ActionControl.routeAction(String action, ActionParameters params)`.
 You don't usually need to care about the `ActionControl` class though as the framework handles using it.

Here's and example of an action handler:

```java
package org.oskari.example;

import fi.nls.oskari.annotation.OskariActionRoute;
import fi.nls.oskari.control.*;
import fi.nls.oskari.log.LogFactory;
import fi.nls.oskari.log.Logger;
import fi.nls.oskari.util.ResponseHelper;

/**
 * Dummy Rest action route
 */
@OskariActionRoute("MyAction")
public class MyActionHandler extends RestActionHandler {

    private static final Logger LOG = LogFactory.getLogger(MyActionHandler.class);

    public void preProcess(ActionParameters params) throws ActionException {
        // common method called for all request methods
        LOG.info(params.getUser(), "accessing route", getName());
    }

    @Override
    public void handleGet(ActionParameters params) throws ActionException {
        ResponseHelper.writeResponse(params, "Hello " + params.getUser().getFullName());
    }

    @Override
    public void handlePost(ActionParameters params) throws ActionException {
        throw new ActionException("This will be logged including stack trace");
    }

    @Override
    public void handlePut(ActionParameters params) throws ActionException {
        throw new ActionParamsException("Notify there was something wrong with the params");
    }

    @Override
    public void handleDelete(ActionParameters params) throws ActionException {
        throw new ActionDeniedException("Not deleting anything");
    }
}
```
When you give an identifier for the annotation (`MyAction` in) `@OskariActionRoute("MyAction")` you select how the framework identifies the handler.
You use `Oskari.urls.getRoute("MyAction")` in the frontend **javascript application** to get the URL for the handler.
Note that the `RestActionHandler` is extending `ActionHandler` and provides methods for different HTTP-methods.
 You can use either or add your own base class.

### OskariViewModifier annotation

Requires the annotation processor `fi.nls.oskari.annotation.OskariViewModifierAnnotationProcessor`.

The annotion can be used for processing request parameters or for example server configuration to modify bundle configurations from what they are in the database to what you want the frontend to receive.

A common example is server environment specific values like links to registration pages etc.
 Instead of saving the env specific URLs to the database, you can inject them to a bundle configuration at runtime from server configuration file.

 Another example is that the request parameters are used for passing requested center coordinates and zoom level for the map in links to the application.
  Instead of using the ones recorded as default values on the database, the configuration is modified by runtime to inject the parameter values to the bundle configuration as initial state.

The modifiers can be accessed through `fi.nls.oskari.view.modifier.ViewModifierManager` but you usually don't need to care about it as the framework handles using it.

There are two more specific ViewModifiers: `BundleHandler` and `ParamHandler`.

#### BundleHandler (ViewModifier)

Annotating a class that extends `BundleHandler` with `@OskariViewModifier("bundle-id")` will modify the bundle that the identifier value refers to at runtime.

#### ParamHandler (ViewModifier)

Annotating a class that extends `ParamHandler` with `@OskariViewModifier("parameterName")` will be called when the URL parameter matching the the identifier value is received.
You can then define what should be modified depending on the value. 
