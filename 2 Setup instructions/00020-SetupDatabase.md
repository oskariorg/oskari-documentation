## Setup database

This document describes how to set up a single database for the oskari-server component.

The standalone Oskari server depends on the availability of PostgreSQL with
PostGIS extension for serving content and authenticating users.

### Database requirements:

The following components are assumed pre-installed:

* [PostgreSQL 11+](http://www.postgresql.org/) (Known to work with 11, 12, 14 mainly affected by the version of FlywayDB-dependency that is used by `oskari-server`. Developed using 14.)
* [PostGIS](http://postgis.net/) (Developed using 3)


### Steps to create the database

Install a supported version of PostgreSQL and PostGIS on your environment.
 The default configurations assume PostgreSQL is installed on `localhost` with the default PostgreSQL port (`5432`).
 This is configurable in `oskari-ext.properties` file (found under `oskari-server/lib/` folder in the [downloadable](https://oskari.org/download) file).

#### Create an empty database with PostGIS extension

The default configurations assume the database name is `oskaridb` (configurable in `oskari-ext.properties` file).

Run the create database SQL in for example psql or pgAdmin (see if they were installed in the PostgreSQL installation package):

```sql
CREATE DATABASE oskaridb
WITH OWNER = postgres
  ENCODING = 'UTF8'
  TABLESPACE = pg_default
  CONNECTION LIMIT = -1;
```

Connect to the database with `\c oskaridb` in psql or by opening the query tool on pgAdmin or similar for the `oskaridb` database.

Run this SQL on the database to add the PostGIS extension:

```sql
CREATE EXTENSION postgis;
```

#### Setup a database user for oskaridb

Run these commands to create default user with all privileges

```sql
CREATE USER oskari WITH PASSWORD 'oskari';
GRANT ALL PRIVILEGES ON DATABASE oskaridb TO oskari;
```

The preconfigured database user in Oskari example application is `oskari` with the password `oskari` (configurable in `oskari-ext.properties` file).

Some versions of PostgreSQL might also need you to run:

```sql
GRANT ALL PRIVILEGES ON SCHEMA public TO oskari;
```
If you see error messages like this when starting the server: `Message: ERROR: permission denied for schema public` you need to add privileges for the schema as well.
For production environments you can define lesser privileges for the database user, however the migration scripts (run on version updates) can create, drop or alter tables on the database so you will need to allow these. It is also possible to run the migrations required on server updates with a user that has more privileges while using another user with lesser prileges for operational use.

#### Application initialization and database content

The empty database will be populated with table structure and example data when the application server is started for the first time using automatic migrations. The database migrations are split into modules. The core module creates and migrates the main database tables used by Oskari. Changes to these need to be done on the oskari-server repository itself and usually can't be skipped by the application.

The example configuration includes a migration module named `app` (in `oskari-ext.properties`) that will populate initial example data for the demo application to get a nice unboxing experience. Changing or replacing the `app` module is the intended way of customizing any application specific migrations and possible initial data and/or users. These application specific migrations also create the default users and their credentials that can be used to login to the example application. These you *must* change or remove when creating your own Oskari-based application.

Note that migrations are run only once so making changes to existing migration files and restarting the application server requires an empty database for migrations to be run again (or removing related rows on the migration status database tables). However the migrations follow versioning through naming so you can add migrations to modify an existing database by naming the migrations in a certain way.

The next step after having the database ready is installing the application that runs these migrations. If you want to skip way ahead and learn how to customize Oskari including populating the database with your own content instead of example content see:
* [Create a custom Oskari-server extension](../7%20Developing%20instructions/00150-HowToCreateACustomOskariServerExtension.md)
