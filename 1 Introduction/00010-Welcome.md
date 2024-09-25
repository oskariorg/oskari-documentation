# Welcome

## Introduction to Oskari

Welcome to Oskari's documentation!

Oskari is an open source Web Map Framework. The aim of Oskari is to help organisations to create better map applications to be used on web browsers and mobile devices. Oskari supports EU's INSPIRE directive and OGC standards.

Oskari enables you to create a map service that is either a full blown geoportal, a highly customized embedded map or something in between.

Oskari enables the user to:
- View spatial data on their web browser
- Combine many different data sources into one map service
- View multiple map layers simultaneously
- Visualizing spatial data
- Embedding user's own maps to other websites as an iFrame

Oskari's appearance, UI and functionalities are customizable. The developers can create their own tools, too.

As an open source project Oskari is developed together. The development is coordinated by National Land Survey of Finland. Oskari is dual-licenced MIT/EUPL 1.2.

Oskari's GitHub repositories are
-  https://github.com/oskariorg/sample-application
-  https://github.com/oskariorg/sample-server-extension
-  https://github.com/oskariorg/oskari-frontend
-  https://github.com/oskariorg/oskari-server
-  https://github.com/oskariorg/oskari-documentation


Note that this document is a work in progress. If you find any outdated information, notice that something is missing or that some sections could be improved, let us know by making an issue in GitHub or a Pull request to the Oskari documentation [repository](https://github.com/oskariorg/oskari-documentation).

If you have further questions or need help with your Oskari instance, see our Contribute and FAQ pages or feel free to reach out to us in [Gitter](https://gitter.im/oskariorg/chat) or the mailing list!

### Technical overview

Here is a brief technical overview of Oskari. More information about frontend and backend can be found in section _2 Application environment_.

Oskari has a separate browser-based frontend and a server-side backend that combine into an Oskari-based service.

**Oskari's frontend** is a JavaScript-based collection of "bundles". A bundle in Oskari is a technical term used to describe a frontend code package that implements a specific functionality. The bundles can be easily used by developers to mix-and-match different functionalities and used as building blocks for their own Oskari-based application. Frontend's GitHub repository can be found [here](https://github.com/oskariorg/oskari-frontend).

**The backend** is a Java web application built with Maven that can be deployed to a Java Servlet container like Tomcat or Jetty. The oskari-server GitHub repository can be found [here](https://github.com/oskariorg/oskari-server) and consists of Maven modules that can be used as building blocks for Oskari-based webapps. Oskari.org provides a Maven repository as an easy way to use these in Oskari-based applications. The sample server extension is a template repository that can be used to quickly start working on a customized backend for Oskari-service and can be found [here](https://github.com/oskariorg/sample-server-extension)

 **The sample application of Oskari's frontend** is a Single Page Application (SPA) that uses the modules from oskari-frontend for creating an Oskari-based application. It is a template repository that can be used to quickly start working on a customized application frontend and can be found [here](https://github.com/oskariorg/sample-application)

The template (sample) repositories are meant to be used as starting points for creating an Oskari-based service with your own selection of functionalities, data, users, layout, colors and other customizations. These can also be used to add new service specific functionalities without forking the Oskari codebase.

See also **the unofficial Oskari bundles created by the Oskari community** [here](https://github.com/oskariorg/oskari-frontend-contrib)

Aside of the frontend and backend, Oskari requires a PostgreSQL/PostGIS database. For caching purposes and messaging in clustered environments Oskari uses Redis. Oskari is built for featuring benefits of a spatial data infrastucture and uses data from standard OGC-services.

Previous versions of Oskari required a GeoServer installation, and even though many organisations that use Oskari have a GeoServer instance too, Oskari does not require it anymore.

The data displayed on the service is fetched from APIs.

### Requirements for developing Oskari

Developer is a person who develops new features or improves existing functionalities in Oskari. An Oskari developer can contribute to Oskari core or Oskari community plugin development.

The requirements for developing the backend and the frontend differ. But which one should you make changes to? As a rule of thumb:
- If you want to customize the way Oskari looks, what elements and tools are being shown and what they look like, you need to develop **the frontend**.
- If you want to make completely new functionalities, you have to make changes to **the backend**.

Basic requirements for developing Oskari:

- Version management: Git
- Frontend: JavaScript, HTML, CSS, SCSS, React, OpenLayers
- Backend: Java, PostgreSQL/PostGIS, Redis, Jetty/Tomcat
- Build infrastructure & tests: Maven/JUnit for server, Webpack/Jest for frontend.
- Understanding about the following software: PostGIS, Jetty/Tomcat, Redis, Nginx/httpd
- Understanding about OGC standards: WMS, WMTS, WFS, WCS

If you are planning to develop Oskari, don't hesitate to contact NLS FI right at the start. The coordinators from NLS are willing to guide your work and give general guidelines for developing work. Remember **to always document your work** and please use the official Oskari documentation as a reference for your own documentation. As an open source project Oskari requires the contribution of the community that uses it, so please make pull requests to the main Oskari repositories, too.