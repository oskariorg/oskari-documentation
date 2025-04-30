### Configuring domain and paths


```properties
# this is used as baseurl for published maps (external url)
oskari.domain=http://localhost:8080

# path for incoming calls to access map
oskari.map.url=/

# url path to call for ajax requests/action routes
oskari.ajax.url.prefix=/action?

# Allow published maps to be loaded from these domains
view.published.usage.unrestrictedDomains = localhost
```

The frontend application version (used as part of path)
```properties
oskari.client.version=dist/2.0.1
```

### Configuring languages

The first one is considered the default language:

```properties
# Supported locales, comma separated and default first
oskari.locales=en_US,fi_FI,sv_SE
```
UI components that ask localized values show fields for these languages.
