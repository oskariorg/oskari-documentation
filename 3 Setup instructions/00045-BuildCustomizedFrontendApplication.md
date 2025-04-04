## Build customized frontend application

This section describes how to replace the prebuilt frontend application with your own version.
This is the intended way of customizing Oskari-based applications.

### Requirements

The following items are required for the development process:

* NodeJS 18+
* [Git client](http://git-scm.com/) (optional)

You will need an environment to run the code in as described on [Setup application server](00030-SetupApplicationServer.md)

Feel free to use the [git conventions](../8 Developing instructions/00115-GitGuidelines.md) used in Oskari development with your own customizations, but it's your app so you can make your own choices.

### Create your application repository

You can use our `sample-application` template for creating a repository that will have your application customizations under your own GitHub user/organization:
https://github.com/new?template_name=sample-application&template_owner=oskariorg

Clone the repository to your own computer with git. You can use any program you are comfortable with to work with git, but on command line you can run:

```sh
git clone https://github.com/oskariorg/sample-application.git
```
Replace `oskariorg` with your own username/organization name and `sample-application` if you changed the repository name for your app.
For simple testing you can also just clone our template repository for tinkering, replacing the existing `sample-application` folder with your cloned repository.

If you don't use git you can also just download the repository code as zip from [GitHub](https://github.com/oskariorg/sample-application/archive/refs/heads/master.zip).

Note! The sample application, including its source code and build, is available in the `sample-application` folder within the [Oskari download package](/download). The frontend application builds are generated in the `dist` folder under the `sample-application` and the prebuilt version is included in the download zip file.

### Build your version of the frontend application

Go to the root folder of the repository (`cd sample-application`) and run the npm command:

```sh
npm run build
```
This creates a new version folder under `sample-application/dist/`.

TODO:
- how to change the folder name
- should we rename sample-application on the download-zip to `frontend-app` or similar?
- how to check that server uses the correct frontend version
- what are the easy changes to see it works
- npm run start (dev mode)
