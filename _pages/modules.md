---
layout: default
title: Modules
permalink: /modules/
---

#Relevant Apigee-127 modules and dependencies

This topic briefly describes the relevant Node.js modules on which an Apigee-127 project depends.

* [apigee-127](#apigee-127)
* [a127](#a127)
* [apigee-access](#apigee-access)
* [skeleton](#skeleton)
* [swagger-editor-for-apigee-127](#swagger-editor-for-apigee-127)
* [swagger-tools](#swagger-tools)
* [usergrid-installer](#usergrid-installer)
* [volos](#volos)
* [a127-magic](#a127magic)
* [volos-swagger](#volosswagger)

##<a ref='apigee-127'></a>apigee-127

The `apigee-127` npm module provides everything you need to create new Apigee-127 projects, including the Swagger editor, Swagger Tools middleware, sample project skeleton, Volos.js, Usergrid, and the `a127` command-line tools.

####Installation
For installation instructions, see "[Installation](https://github.com/apigee-127/a127-documentation/wiki/Installation)".

####Documentation

The main source of documentation for Apigee-127 and related components is the [Apigee-127 documentation wiki(https://github.com/apigee-127/a127-documentation/wiki) on GitHub.

You can find additional documentation for related components, such as Volos.js in README files on GitHub. We provide links below.

## <a ref='a127'></a>a127

The `a127` module includes a set of command-line tools for creating and managing Apigee-127 projects.

####Installation

The a127 command-line tools are installed with Apigee-127. You can find the [a127 project](https://github.com/apigee-127/a127) on GitHub.

#### Documentation

[a127 command-line tool reference](https://github.com/apigee-127/a127/blob/master/README.md)

## <a ref='apigee-access'></a>apigee-access

The `apigee-access` module allows Node.js applications running on the Apigee Edge platform a way to access Apigee-specific functionality. You can use this module to:

* Access and modify "flow variables" within the Apigee message context.
* Retrieve sensitive data from the the secure store.
* Use the built-in distributed cache.
* Use the built-in distributed quota service.
* Use the OAuth service.

####Installation

Standard npm install. Installed with `apigee-127`.

####Documentation

See the `apigee-access` [README](https://github.com/apigee/apigee-access/blob/master/README.md) on GitHub.

## <a ref='skeleton'></a>skeleton

A basic, "hello world" Apigee-127 project. This project automatically cloned when you create a new Apigee-127 project by executing `a127 project create`.

#### Installation

This project is [cloned from GitHub](https://github.com/apigee-127/project-skeleton) when you create a new Apigee-127 project.

#### Documentation

See the Apigee-127 "[Quick start](https://github.com/apigee-127/a127-documentation/wiki/Quick-start)" to see how easy it is to get a new Apigee-127 API project up and running.

## <a ref='swagger-editor-for-apigee-127'></a>swagger-editor-for-apigee-127

The Swagger Editor for Apigee-127 lets you design your API specification and interactively preview its documentation for your Apigee-127 API project.

####Installation

Standard npm install. Installed with Apigee-127.

####Documentation

See "[Understanding Swagger](https://github.com/apigee-127/a127-documentation/wiki/Understanding-Swagger)" on the Apigee-127 doc wiki for details about the editor.

## <a ref='swagger-tools'></a>swagger-tools

Middleware for Node.js including Message Validation, Authorization and Routing.

####Installation

Standard npm install. Installed with Apigee-127.

####Documentation

See the `swagger-tools` [README](https://github.com/apigee-127/swagger-tools) on GitHub.

See also the [Twitter search example on Apigee-127](https://github.com/apigee-127/example-project/blob/master/README.md) to see how the `x-swagger-router-controller` Swagger extension, based on `swagger-tools`, is used for API endpoint routing in an Apigee-127 project.

#### Swagger Tools middleware components

Swagger tools provides these middleware comnponents. They provide services for message validation, authorization, and routing.

* swagger-metadata
* swagger-router
* swagger-validator

## <a ref='usergrid-installer'></a>usergrid-installer

This module is designed to download a the version of Usergrid assigned in the package.json usergrid section. It was designed to allow the "embedding" of Usergrid within Apigee-127, but could potentially be used by other apps.

#### Installation

Standard npm install. Installed with Apigee-127.

####Documentation

See the `usergrid-installer` [README](https://www.npmjs.org/package/usergrid-installer) on npmjs.org.

##<a ref='volos'></a>volos

Volos.js is a set of [Connect](https://github.com/senchalabs/connect#readme)-compatible services written in Node.js that make it possible for developers to easily add common API design patterns like security and traffic management to their code.

####Installation

Standard npm install. Installed with Apigee-127.

####Documentation

See "[Understanding Volos.js](https://github.com/apigee-127/a127-documentation/wiki/Understanding-Volos.js)" on the Apigee-127 doc wiki.

See also the [README](https://github.com/apigee-127/volos) file for `apigee-127/volos` on GitHub.

#### Modules included with Volos.js

Volos includes a collection of modules for adding these features to an API:

* Caching: Response caching that can be configured by URI or custom function.

* Analytics: Analytics that can published to Apigee Edge Analytics.

* OAuth 2.0: Full OAuth 2.0 Server or OAuth 2.0 proxy to Apigee Edge.

* Quota: Quota on a per-API, per-resource, per-header or per-parameter basis, or with a customized function.

* Management: Manage developers and applications on Apigee Edge and Redis.

####Volos.js module documentation

Here are links to the primary documentation for the main Volos.js modules:

* [Caching doc](https://github.com/apigee-127/volos/tree/master/cache/common)
* [OAuth 2.0 doc](https://github.com/apigee-127/volos/tree/master/oauth/common)
* [Quota doc](https://github.com/apigee-127/volos/tree/master/quota/common)
* [Management doc](https://github.com/apigee-127/volos/tree/master/management/common)

## <a ref='a127magic'></a>a127-magic

This is a utility module that installs all of the dependencies required for an Apigee-127 project. If you "require" this module in your Apigee-127 app, all relevant Apigee-127 modules will be available in your project code.

### Installation

Standard npm install. Included when you create a new Apigee-127 project.

###Documentation

See the [README](https://www.npmjs.org/package/a127-magic) file on npmjs.org.


## <a ref='volosswagger'></a>volos-swagger

Provides Swagger vendor extenstions that let you configure Volos.js features (like caching and OAuth) in your project's `swagger.yaml` API specification file.

##Installation

Standard npm install. This module is included when you create a new Apigee-127 project (it is a dependency of a127-magic).

##Documentation

See the [README](https://www.npmjs.org/package/volos-swagger) on npmjs.org.


