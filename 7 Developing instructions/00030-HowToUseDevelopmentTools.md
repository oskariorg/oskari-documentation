# Developing instructions

## How to use development tools

#### GitHub, the collaboration platform

The Oskari source code is stored in several separate Git repositories hosted under the [oskariorg](https://github.com/oskariorg) organization on GitHub.

The source code projects on GitHub also facilitate collaboration through hosting the project's
external issue trackers. The issue tracking for Oskari has been centralized to the [oskari-documentation](https://github.com/oskariorg/oskari-documentation/issues) repository as we have different repositories for the server and frontend for example and it's not always easy to know which repository the issue relates to when submitting one.

Source code contributions to the project should be sent in as GitHub Pull Requests to the relevant
repository's "develop" branch, according to the Git Flow branch policies.

Pull Requests and the merging thereof are amply documented in the GitHub documentation.

You will need to install `git` to actively participate to the development process.

#### Collaboration activities

Register a personal account at GitHub.

Fork the `oskariorg/oskari-frontend` and/or `oskariorg/oskari-server` repositories, and check out the source code
from your forks to your local computer when participating in the development for the Oskari-repositories.
 If you are not participating in developing Oskari itself, you can just use the `oskariorg/sample-application` and `oskariorg/sample-server-extension` template repositories to start customizing your own Oskari-based application.

Read Atlassian's [Git Flow documentation](https://www.atlassian.com/git/tutorials/comparing-
workflows/gitflow-workflow).

Create a new feature branch with `git checkout -b feature/[name]` and publish/push it into your own GitHub
repository as a feature branch.

Please make sure to configure your Git tooling so that any new files, or changes to old files, use
only UNIX End-Of-Line markers, and never DOS/Windows End-Of-Line markers.

#### Oskari source code

The `github.com/oskariorg/oskari-frontend` repository contains the frontend JavaScript and CSS source code.
 It assumes being served from webserver path /Oskari with a capital O so you can checkout the repository under the name `Oskari` or configure the webserver to do this. The [download package with Tomcat](/download) is configured properly for this.

The `github.com/oskariorg/oskari-server` contains the Oskari serverside code.

#### Licensing

The Oskari source code is dual licensed with [EUPL](https://github.com/oskariorg/oskari-server/raw/master/LICENSE-EUPL.pdf)
and the [MIT](https://github.com/oskariorg/oskari-server/blob/master/LICENSE-MIT.txt) licenses, which are made available in the source code
repositories.

Not all of the source code files contain a license header.

* * *

#### Software development tools

#### Java development tools

A large part of the Oskari software development process consists of adding new features to the
backend server, using the Java development environment.

The principal tools necessary for Java development are:

  * Java Development Kit (JDK) 17
  * Maven (3.6.3)

You will need to install those tools in the manner relevant to your development platform,
principally Linux, Mac OS X or Windows with WSL.

For Java files, the project uses indentation of 4 spaces, and the so-called Sun-Style with the
exception of maximum line length of 120 characters.

#### Java development tools, validation

A properly installed Java Development Kit should report version 17, or some update version of it, in
response to the command line command
```sh
java -version
```
Maven, on the other hand, should report both its own version, as well as the Java version it is configured to use, in
reponse to the command line command

```sh
mvn --version
```

#### JavaScript and HTML development tools

The Oskari frontend consists of JavaScript code, CSS with resources, as well as internationalization and
localization data files.

The frontend JavaScript and CSS files are stored in the Git version control repository in
structured, and easy to manage hierarchies. These are later compiled into small deployment packages
using Node.js tooling.

Therefore, a developer must install a platform-appropriate Node.js execution and runtime
environment, along with the NPM package manager.

#### Databases

The primary Oskari server components store their data on PostgreSQL databases, and perform some
of the spatial analysis of the data on the database layer, using the PostGIS extension.

Some of the backend functionality cache data on a Redis server.

Again, these components should be installed locally, in a platform-appropriate manner. On Linux,
using `apt-get` or `yum`, on Mac OS X using Homebrew, and on Windows, installer EXE or MSIs.

The PostGIS extension must be separately enabled on each database it is invoked on. Please follow
the relevant documentation.

To validate a successful installation of these tools, use either the `psql` command line tool, or a
GUI tool, if you prefer, to connect to the PostgreSQL server.

To validate a successful installation of PostGIS, use your connection to the PostgreSQL database to
execute the PostGIS version query

```sql
SELECT PostGIS_full_version();
```

Your Redis installation can be checked with the command line command

```sh
redis-cli info
```

#### Application server - Tomcat

The Oskari server template application builds into a WAR file executed inside a servlet container.

A good servlet container for development and production is Tomcat 10.1, but the application can just as
well be run on other servers like Jetty as well.

The default application configuration settings reside in the `oskari.properties` configuration file in the
WAR `classes`-directory. These can be overridden with oskari-ext.properties file in the servers classpath (for example `lib` folder under `{tomcat.base}`).

The servlet in the WAR will attempt to connect to a pre-existing PostgreSQL database using the
settings specified in the `oskari(-ext).properties` file.


* * *

#### Configuring and building the service

#### Database creation and configuration

The PostgreSQL tools `createuser` and `createdb` should be used to create a new database user
`oskari` with a password. These settings should also be transferred to your `oskari-ext.properties`.

After creating the database and starting the server the application creates the database schema and populates it with example content.

The documentation for the process of upgrading the database with [Flyway migrations](../4%20Server%20application/00040-DatabaseMigrations.md) is also worth going
over.

#### Building the backend

The development build for the backend is compiled and run using Maven, as described [in the
development documentation](../2%20Setup%20instructions/00040-BuildCustomizedServerApplication.md).

* * *
