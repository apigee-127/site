---
layout: default
title: What is Apigee 127?
permalink: /what-is-a127/
---

# What is Apigee 127?

Apigee 127 provides the tools you need to design and build Enterprise-class APIs entirely in Node.js and deploy them on any Node.js runtime system.

In this topic, we'll introduce the parts of Apigee-127 and discuss it's unique model-first approach to API development.

* [Quick overview](#overview)
* [The Model-first Apigee 127 programming approach](#programming_model)
* [Learn from working code samples](#samplescode)
* [Watch Apigee-127 videos](#videos)
* [Get help](#gethelp)

## <a name="overview"></a>Quick overview of Apigee 127

Apigee-127 lets you model, build, and test your API model **first** in the intuitive, interactive Swagger editor. When you're happy with your design, you can focus on writing custom controller code (in Node.js) for each of your APIs operations.

These are the main parts of Apigee-127.

* A [Swagger 2.0 Editor] [swagger-editor] running locally, built for the Swagger Community by Apigee. The editor lets you design, build, test your API with easy-to-learn YAML configuration.

![alt text](https://raw.githubusercontent.com/apigee-127/a127-documentation/master/a127/images/swagger-editor.png)

* [Swagger Tools] [swagger-tools-github] middleware for Node.js including Message Validation, Authorization and Routing. Swagger Tools extensions provide the glue between the Swagger editor configuration and your API implementation.

* [Volos.js] [volos-github] middleware adds value to your API with features such as Caching, Quota and OAuth 2.0. You can add Volos.js features directly in Node.js code, or, with Swagger Tools extensions, configure them directly in YAML in the Swagger editor. For example:

    ```yaml
      x-volos-resources:
        cache:
              provider: volos-cache-memory
              options:
                  name: weather-cache
                  ttl: 60000
    ```

* [Apigee 127 Command-Line Interface] [a127-cli] for managing project lifecycle. Create and start projects, manage Apigee-127 accounts, deploy projects, and more. For example, to create a new project:

    `a127 project create`

* An easy-to-manage local [Usergrid] [usergrid] runtime and portal

* A Node.js version of [apigeetool] [apigeetool-github] for deploying your application to Apigee Edge

## <a name="programming_model"></a>The Model-first Apigee 127 programming approach

The focus of Apigee 127 is using a standard model for building APIs.  From this standard model, Apigee 127 middleware can infer a lot from your API, such as:

* What type of resources are needed to make it run
* What authorization scopes should be applied to which paths?

Combining the Node.js `swagger-tools` and Volos.js middleware modules, Apigee 127 enables you to quickly build robust, high quality APIs by offloading a lot of the work needed to do so.  From the Swagger 2.0 model, Apigee 127 creates the server-side flows and middleware specified in the Swagger specification, leaving the core logic of the API up to API developers.

The programming flow for an Apigee 127 project looks like this:

![Alt text](https://raw.githubusercontent.com/apigee-127/a127-documentation/master/a127/images/programming-model.png)

* Define the Swagger Model using the Swagger 2.0 Editor included with Apigee-127.

* Annotate your resources and operations in the Swagger 2.0 model with the `x-swagger-router-controller` extension to define the name of the Controller that implements the logic behind the operation.  For example:

```yaml
paths:
  /hello:
    x-swagger-router-controller: "hello_world"
```

* Utilize the `operationId` property for your operations in the Swagger 2.0 Model
```yaml
    get:
      description: "Returns 'Hello' to the caller"
      operationId: "hello"
```

* Optionally use the Volos.js Swagger Extensions to define Caching, Quota and OAuth configuration:
```yaml
x-volos-resources:
  cache:
    provider: volos-cache-memory
    options:
      name: name
      ttl: 10000
  quota:
    provider: volos-quota-memory
    options:
      timeUnit: minute
      interval: 1
      allow: 2
  oauth2:
    provider: volos-oauth-apigee
    options:
      key: *apigeeProxyKey
      uri: *apigeeProxyUri
      validGrantTypes:
        - client_credentials
        - authorization_code
        - implicit_grant
        - password
      passwordCheck: passwordCheck
```

* Optionally apply Volos.js functions to individual operations to add caching, quota and OAuth security to your API. You can integrate `volos` features directly into your Swagger model using the Volos.js Swagger Extensions or implement them programmatically in the Node.js app.  Example:
```yaml
  /twitter:
    x-swagger-router-controller: twitter
    x-volos-authorizations:
      oauth2: {}
    x-volos-apply:
      cache: {}
      quota: {}
    get:
       ...
```

* Behind the scenes, Apigee 127 wires up your app, routing HTTP requests to specific Node.js controller files.

* At runtime the `swagger-router` will validate & authorize the request before sending it to the `hello` operation of the `hello_world` controller which is found in `{project_home}/api/controllers/hello_world.js`.  By default the swagger-router looks for controllers in `[project_home]/api/controllers` but this can be overridden in the project.

* If configured, Volos.js Middleware will handle Caching, Quota and/or OAuth authorization

* Finally, your controller logic will be invoked according to the `x-swagger-router-controller` specified for the resource path and the `operationId` of the corresponding operation.  By default the Controller should be in `[project_home]/api/controllers/[x-swagger-router-controller].js`

* You can develop and test your API locally using the command `a127 project start`

* Once you are ready to deploy your API it can run anywhere Node.js can run anywhere:
    * Apigee Edge
    * custom servers in your datacenter
    * Other PaaS providers that support Node.js such as [Heroku][heroku] or [Amazon Elastic Beanstalk][aws-eb].

** However, to take advantage of the full suite of Apigee's value-added services the API should be deployed there.


## <a name="samplescode"></a>Learn from working code samples

* [Quick start](https://github.com/apigee-127/a127-documentation/wiki/Quick-start) -- Read or work through a basic end-to-end "hello world" example. Build up the basic project by adding caching, analytics, OAuth 2.0 security, and quota enforcement to the API.

* [Twitter search example with Apigee-127](https://github.com/apigee-127/example-project/blob/master/README.md) -- Learn from a working reference implementation: an Apigee-127 API that calls the Twitter Search service on the back-end.

* [Sample applications](https://github.com/apigee-127/a127-samples) -- Learn from working samples including basic and advanced weather APIs built with Apigee-127.

## <a name="videos"></a>Watch Apigee-127 videos

* NEW WEBCAST! [Build APIs in Node.js & Swagger 2.0 with Apigee-127](https://blog.apigee.com/detail/build_apis_in_nodejs_swagger_20_with_apigee_127_webcast_podcast)

* [Introduction to Apigee-127](http://www.youtube.com/watch?v=JD4YFacOF8o) -- A short video overview of Apigee-127.

* [Hands-on Apigee-127 demo](https://www.youtube.com/watch?v=So91lkdzDyU) -- See how easy it is to create, edit, run, and deploy a new Apigee-127 project.

* [Basic weather API demo](http://vimeo.com/105493716) -- A demo showing an API calling another API using Apigee-127.

* [Advanced Apigee-127 project walk-through](http://vimeo.com/105500743) -- Walks through the advanced Apigee-127 example project that's [available on GitHub](https://github.com/apigee-127/a127-samples).

* [Apigee-127 with Apigee Analytics](http://vimeo.com/105495153) -- Shows how to use Apigee Analytics as a service with Apigee-127.

## <a name="gethelp"></a>Get help

Need help using Apigee-127? Have an issue to report? See the [Reporting issues and getting help](https://github.com/apigee-127/a127-documentation/wiki/Submitting-Issues).

[aws-eb]: http://aws.amazon.com/elasticbeanstalk/
[heroku]: https://www.heroku.com/
[volos-github]: https://github.com/apigee-127/volos
[a127-cli]: https://github.com/apigee-127/a127
[swagger-editor]: https://github.com/wordnik/swagger-editor
[swagger-tools-github]: https://github.com/apigee-127/swagger-tools
[apigeetool-github]: https://github.com/apigee/apigeetool-node/tree/master
[usergrid]: http://usergrid.incubator.apache.org/