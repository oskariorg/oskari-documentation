## Location search

Oskari has search functionality that can be configured to use custom search services as a backends to power the search. An implementation for OpenStreetMap nominatim service is included as an example adapter.

**The location search** is a one-field search which can return search results from multiple sources, such as placename, address, cadastral parcel or similar services. The service access parameters are configurable in the backend. If one needs to search for cadastral parcels or any other type of search result better suited to their application, a search backend that supports this can be configured.

### Search channel

Describe the search channel concept.

### WFS Search channels

There is an user interface for admin users that allows configuring WFS-services as backend for the search. This can be used to provide for example cadastral parcels as search results.
