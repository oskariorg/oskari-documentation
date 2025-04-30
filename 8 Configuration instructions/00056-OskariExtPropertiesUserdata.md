### Configuring storing data


```properties
# initialized the user content like myplaces baselayers with this srs 
# layers are also updated by setup.war if used to generate GeoServer config
oskari.native.srs=EPSG:3857

```

#### User management

```properties
# UserService implementation - create own implementation to integrate into actual systems and provide feedback for missing interface methods.
oskari.user.service=fi.nls.oskari.user.DatabaseUserService
# true to NOT allow user edits from frontend by admin (for admins benefit since external users data is usually overwritten on login)
#oskari.user.external=false
```
