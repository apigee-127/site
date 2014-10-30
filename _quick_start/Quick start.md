---
layout: default
title: About
permalink: /quick-start/about/
---

The purpose of this Quick Start is to show you how easily and quickly you can get a simple API up and running using Apigee-127.

* [Get an API up and running](#upandrunning)
* [Open the Swagger editor](#openeditor)
* [Next steps](#nextsteps)

### <a name="upandrunning"></a>Get an API up and running

First, we create a new Apigee-127 project and test the default "hello world" API.

1. Follow the steps here: [Installation](https://github.com/apigee-127/a127-documentation/wiki/Installation)
3. Create a root folder for your Apigee-127 projects and cd to that folder.
4. Execute: `a127 project create hello-world`.  This will clone a project skeleton from GitHub into a directory from the working directory named `hello-world`.  It will then run `npm install` to download dependencies for the project.  [Windows Users please see the note below regarding npm](#windows-note)
5. Change to the `hello-world` directory by typing `$ cd hello-world`
7. Type `$ a127 project start` to start your API.  You now have an API running with Apigee-127!
9. In another terminal, run: `$ curl http://localhost:10010/hello?name=Me`.  You should see the response `Hello, Me`.

That's it - You have now created, started and tested your first API project with Apigee-127!

### <a name="openeditor"></a>Open the Swagger editor

Before moving on to another topic, start the Swagger editor. It loads in the Swagger specification file for the quick start API and lets you design and test your API interactively.

From the root folder of the Apigee-127 project, execute:

`a127 project edit`

![alt text](https://raw.githubusercontent.com/apigee-127/a127-documentation/master/a127/images/swagger-editor.png)
*The Swagger editor for Apigee-127*

You'll start working with the editor in the next Quick Start topic: [Add a Quota][quickstart-add-qouta].

## <a name="nextsteps"></a>Next steps

Next, we'll show how to build out the API with important features like caching, quota management, OAuth 2.0, and more.

* [Add a Quota] [quickstart-add-qouta]
* [Add a Path] [quickstart-add-path]
* [Implement a Controller] [quickstart-implement-controller]
* [Add Caching] [quickstart-add-caching]
* [Add Apigee OAuth] [quickstart-add-oauth-apigee]
* [Add Apigee Analytics] [quickstart-add-analytics-apigee]
* [Deploy to Apigee Edge] [quickstart-deploy-edge]

### <a name='windows-note'></a>Windows Users
For some versions of npm on Windows will have problems on the `npm install` step of `a127 project create`.  They are related to a `debug` module on npm not being managed properly.  The following steps should resolve this issue:

1. In the project directory, execute the following commands:
  1. `npm install yamljs`
  2. `npm install debug`
  3. `npm install swagger-tools`

Now, when you run `a127 project start` your project should start successfully.


[skeleton]: https://github.com/apigee-127/project-skeleton
[fork]: https://help.github.com/articles/fork-a-repo
[pull]: https://help.github.com/articles/using-pull-requests
[quickstart-add-qouta]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Add-Quota
[quickstart-add-path]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Add-a-New-Path
[quickstart-implement-controller]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Implement-a-Controller
[quickstart-mock-mode]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Operating-in-Mock-Mode
[quickstart-add-caching]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Add-Caching
[quickstart-deploy-edge]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Deploy-To-Apigee-Edge
[quickstart-deploy-heroku]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Deploy-To-Heroku
[quickstart-deploy-beanstalk]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Deploy-To-EB
[quickstart-add-oauth-apigee]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Add-Apigee-OAuth
[quickstart-add-analytics-apigee]: https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Add-Apigee-Analytics