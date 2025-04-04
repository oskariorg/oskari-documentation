## Build customized server-side application

This section describes how to replace the prebuilt server side webapp with your own version.
This is the intended way of customizing Oskari-based applications.

### Requirements

The following items are required for the development process:

* JDK 17
* [Maven 3+](http://maven.apache.org/) (developed using 3.6.3)
* [Git client](http://git-scm.com/) (optional)

You will need an environment to run the code in as described on [Setup application server](00030-SetupApplicationServer.md)

Feel free to use the [git conventions](../8 Developing instructions/00115-GitGuidelines.md) used in Oskari development with your own customizations, but it's your app so you can make your own choices.

### Create your application repository

You can use our `sample-server-extension` template for creating a repository that will have your application customizations under your own GitHub user/organization:
https://github.com/new?template_name=sample-server-extension&template_owner=oskariorg

Clone the repository to your own computer with git. You can use any program you are comfortable with to work with git, but on command line you can run:

```sh
git clone https://github.com/oskariorg/sample-server-extension.git
```
Replace `oskariorg` with your own username/organization name and `sample-server-extension` if you changed the repository name for your app.

For simple testing you can also just clone our template repository for tinkering, but **BEWARE** the template repository is intended to be _a starting point_ and it will be updated in ways where the migrations and initial content will be modified in place (skipping the migration history like is usually done for maintained _apps_). This means that going forward you can't simply merge released changes from the template repository to your application repository using git so it's best to create a copy and not a fork of the sample-server-extension for your own app.

If you don't use git you can also just download the repository code as zip from [GitHub](https://github.com/oskariorg/sample-server-extension/archive/refs/heads/master.zip).

### Build your version of the server application

Once you have the server application repository cloned (or downloaded and unzipped) you can use Maven to build your own version of the `oskari-map.war`.

Go to the root folder of the repository (`cd sample-server-extension`) and run the Maven command:

```sh
mvn clean install
```

This will create the `oskari-map.war` file under `webapp-map/target`.

You can replace the `oskari-map.war` from the Oskari download package with this newly created file and restart the server.

### Simple server modifications

These are some easy to test modifications to see that your server application has changed:
- [JSP-files](https://github.com/oskariorg/sample-server-extension/tree/master/webapp-map/src/main/webapp/WEB-INF/jsp) under `webapp-map/src/main/webapp/WEB-INF/jsp` has the base HTML in them. You can simply change the value of `<title>` tag for example.
- [JSON-files](https://github.com/oskariorg/sample-server-extension/tree/master/app-resources/src/main/resources/json/apps) under `app-resources/src/main/resources/json/apps` select the functionalities (Oskari "bundles") that will be used on the application (Note! changing these will require an empty database to see changes)
- [Flyway migrations](https://github.com/oskariorg/sample-server-extension/tree/master/app-resources/src/main/resources/flyway/app) under `app-resources/src/main/resources/flyway/app` and `app-resources/src/main/java/flyway/app` (Note! changing these will require an empty database to see changes)

**Note!**  When you change something in the code, you need to recompile/build a new version of `oskari-map.war` and deploy the new version on the server.
If you change anything on the initial data/migrations you need to drop the database and create a new empty one. To migrate the content from it's initial state you can _add new migrations_ that will be run on an existing database, but modifying ones that have already been executed on the database will not be rerun on a non-empty database.
