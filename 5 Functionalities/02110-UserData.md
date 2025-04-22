## Create data using My Data (requires backend)

The end-user of Oskari instance can create their own spatial data in the main map window. For this purpose, Oskari supports vector data, which means that points, lines and areas can be created and saved. Multigeometries and creation of holes is also supported.

The users can:
- Create multiple layers
- Configure the symbology for each layer separately
- Edit the description of map view and add links to external resources

## Import data (requires backend)

Users can import their own datasets to Oskari as zipped files. Supported formats are:

- Shapefile
- Mapinfo MID/MIF
- GPX trace
- KMX (zipped KML)

### User removal

User content (myplaces, saved views, embedded maps, userlayers, indicators) is removed from the database with the user.
 The content removal is done programmatically by searching for instances of [UserContentServices](https://github.com/oskariorg/oskari-server/blob/master/service-base/src/main/java/fi/nls/oskari/service/db/UserContentService.java)
 with `@Oskari` annotation. You can search the `oskari-server` codebase for examples of this if you need to add additional cleanup for user removal.

**Note!** Removing a user from the database directly will not remove all content related to the user!
