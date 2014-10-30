---
layout: default
title: Swagger
permalink: /swagger/
---

# <a name="understandingthespec"></a>Understanding the Swagger specification file

* [Intro](#intro)
* [The default Swagger specification file listing for Apigee-127 projects](#default)
* [Apigee-127 Swagger specification reference](#reference)

## <a name="intro"></a>Intro

When you execute `a127 project create myproject`, a default Swagger model is placed in `myproject/api/swagger/swagger.yaml`. This model conforms to the [Swagger 2.0 specification](https://github.com/reverb/swagger-spec/blob/master/versions/2.0.md).

>Note: For a quick intro to swagger, see "[Understanding Swagger](https://github.com/apigee-127/a127-documentation/wiki/Understanding-Swagger)".

## <a name="default"></a>The default Swagger specification file listing for Apigee-127 projects

Here is the entire `swagger.yaml` file that is provisioned for a new Apigee-127 project:

```yaml
swagger: 2.0
info:
  version: "0.0.1"
  title: "Skeleton App"
host: "localhost"
basePath: "/"
schemes:
  - "http"
  - "https"
consumes:
  - "application/json"
produces:
  - "application/json"
x-volos-resources: {}
paths:
  /hello:
    x-swagger-router-controller: "hello_world"
    x-volos-authorizations: {}
    x-volos-apply: {}
    get:
      description: "Returns 'Hello' to the caller"
      operationId: "hello"
      produces:
        - "application/json"
      parameters:
        -
          name: "name"
          in: "query"
          description: "The name of the person to whom to say hello"
          required: false
          type: "string"
      responses:
        200:
          description: "Success"
          schema:
            $ref: HelloWorldResponse
        default:
          description: "Error"
          schema:
            $ref: ErrorResponse
definitions:
  HelloWorldResponse:
    required:
      - "message"
    properties:
      message:
        type: "string"
  ErrorResponse:
    required:
      - "message"
    properties:
      message:
        type: "string"
```
##<a name="reference"></a>Apigee-127 Swagger specification reference

The Swagger file includes a number of standard Swagger 2.0 specification elements. You can read about them in the [Swagger 2.0 specification](https://github.com/reverb/swagger-spec/blob/master/versions/2.0.md).

Here's a brief description of the elements in the Apigee-127 Swagger file:

*  **swagger: 2** - (Required) Identifies the version of the Swagger specification (2.0).

*  **info:** - (Required) Provides metadata about the API.

*  **host:** - (Optional) The host serving the API. By default, a new project connects to a server running locally on port 10010.

*  **basePath:** - (Optional) The base path on which the API is served, which is relative to the host.

*  **schemes:** - (Optional) A list of transfer protocol(s) of the API.

*  **consumes:** - (Optional) A list of MIME types the APIs can consume.

*  **produces:** - (Optional) A list of MIME types the APIs can produce.

*  **x-volos-resources:** - (Optional) A custom Swagger extension for the [volos-swagger](https://github.com/apigee-127/volos/tree/master/swagger) module. The volos-swagger module lets you add  Volos.js features like quotas, caching, and OAuth 2.0 to your API by configuring them in the Swagger file. For more information, see the [volos-swagger README file](https://github.com/apigee-127/volos/tree/master/swagger).

*  **paths:** - (Required) Defines the available operations on the API. You'll spend most of your time configuring the paths part of the file. You can read about the path element in the [Swagger 2.0 specification](https://github.com/reverb/swagger-spec/blob/master/versions/2.0.md). In general, the paths section specifies an operation's verb (like `get`), the endpoint for an API operation (like `/hello`), query parameters, and responses.

* **definitions:** - (Optional) These represent the structure of complex objects such as request and response bodies. For example, you might have a collection of `/users` that returns an array of `user` objects. You would describe these with two definitions: 1) to describe the `User` object, and 2) the definition of the `Users` array. Swagger uses [JSON-schema](http://json-schema.org/).

In the Apigee-127 Swagger file, the paths section also includes these custom extensions:

* **x-swagger-router-controller:** - (Optional) This extension specifies the name of the controller file (hello_world.js) that will execute when this API operation is called. Controller files reside in `apis/controllers` in your Apigee-127 project. This extension is provided through the [`swagger-tools`](https://github.com/apigee-127/swagger-tools) middleware module, which is included when you require the `a127-magic` module in your main Node.js app.

* **x-volos-authorizations:** - (Optional) This extension applies Volos.js authorization to the API operation. You can use this extension to add OAuth 2.0 security, for example. For more information, see the [volos-swagger README file](https://github.com/apigee-127/volos/tree/master/swagger).

* **x-volos-apply:** - (Optional) This extension applies specified volos modules, and they use the configurations specified in the x-volos-resources extension, described previously. For more information, see the [volos-swagger README file](https://github.com/apigee-127/volos/tree/master/swagger).

Other custom Apigee extensions to the Swagger model:

* **x-a127-config:** Use this extension to configure authentication values required to run your API in conjunction with Apigee Edge. Lets you configure the `apigeeProxyKey` and `apigeeProxyUri`. The `apigeProxyKey` is used for authentication on Edge, and the `apigeeProxyUri` is the Uri location of the `apigee-remote-proxy` endpoint. These values are set when you create an Apigee account for your Apigee-127 project (`a127 project create myaccount`). For example:

    ```yaml
    x-a127-config:
        apigeeProxyKey: &apigeeProxyKey CONFIGURED
        apigeeProxyUri: &apigeeProxyUri CONFIGURED
    ```

