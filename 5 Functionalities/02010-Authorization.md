## Role-based authorization

Access to different functionalities and data can be granted with user roles.

## Map layer permissions in the database

The maven module under `service-permissions` provides a generic permission handling database. Two db tables (`oskari_resource` and `oskari_resource_permission`) are used to mostly control access permissions to map layers, but the implementation is generic and could be used to control bundle access etc.

### Example: grant view permission for maplayer

We want to grant access for guest users to see a maplayer:

* assume guests have role with id `10110` (configurable in `UserService` implementation)

* assume layer id is `2`

The maplayer needs to be registered as a resource for permissions db (since the permission implementation is generic).

#### oskari_resource

| id | resource_type | resource_mapping      |
|----|---------------|-----------------------|
| 1  | 'maplayer'    | '2'                   |


* `resource_type` is a string reference to another table for example in this case `oskari_maplayer`.
* `resource_mapping` is a table specific reference to the resource in question. For maplayers it's the layer `id` as a string.

After the layer has been added as a resouce we can use it to set the actual permissions for that resource.

#### oskari_resource_permission

| id | resource_id | permission    | external_type | external_id |
|----|-------------|---------------|---------------|-------------|
| 1  | 1           | 'VIEW_LAYER'  | 'ROLE'        | '10110'     |


* `resource_id` is reference to a row in `oskari_resource` table
* `permission` is a constant value of the type of permission in question

* `external_type` is a reference link for the `external_id`. Usually permissions are mapped for user roles so you the value is then `ROLE` which means `external_id` is a role id. Another possible external_type is `USER` which means `external_id` is a user id, but this isn't widely used or might not be supported in all cases. Recommented use for now is to only use `ROLE` mappings.

#### Permission types

* `VIEW_LAYER`: Permission to view a layer on map and geoportal listing
* `PUBLISH`: Permission to use the maplayer as a layer when creating an embedded map
* `VIEW_PUBLISHED`: Permission to view a layer in a published/embedded maps/links, but it is not listed on geoportal
* `DOWNLOAD`: Permission to download a vector layer properties table as a CSV/Excel file from the geoportal
* `EDIT_LAYER`: Permission to modify a layer content (not enabled by default, but content-editor functionality uses this)

#### Example: grant permission to add layers (for non-admin user role)

Adding layers is currently a generic permission (not mapped to data producer or similar). We need to add a resource for "generic-functionality".

##### oskari_resource

| id | resource_type | resource_mapping              |
|----|---------------|-------------------------------|
| 2  | Bundle        | generic-functionality         |

##### oskari_resource_permission

| id | oskari_resource_id | permission    | external_type | external_id |
|----|--------------------|---------------|---------------|-------------|
| 3  | 2                  | ADD_MAPLAYER  | ROLE          | 10110       |


#### Admin user interface for layer permissions

There is an `admin-permissions` bundle with which admins can set layers' permissions. Check [adding bundles](/documentation/backend/adding-bundles) for how to add the bundle.
With that bundle, admin users can set permissions based on user roles for any layers through the user interface.
