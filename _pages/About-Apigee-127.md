---
layout: default
title: About Apigee 127
permalink: /about/
root: true
---

## About Apigee 127

### The programming model

The programming flow looks like this:

![enter image description here][2]

* Use the Swagger editor to define, test, and document your API model. Apigee 127 Node.js modules validate and consume the resulting Swagger definition.

* Behind the scenes, Apigee 127 wires up your app, routing HTTP requests to specific Node.js controller files.

* Then, you implement your API logic in those controller files.

* In addition, you can add a Volos layer to your app to perform caching, quota, and OAuth security for your API.


### API running in conjunction with Apigee Edge

Running locally, your API make calls to Apigee Edge to handle activities like caching, quota management, and OAuth. Volos provides the glue that binds together your API implementation and Apigee Edge.

![Alt text](https://raw.githubusercontent.com/apigee-127/a127-documentation/master/a127/images/with-edge.png)

1. The app makes an API call and the endpoint is the Apigee agent (Volos).
2. Volos calls Apigee Edge to perform OAuth, Quota, or Caching. Volos only sends metadata to Edge, not the API payload.
3. Edge returns a response indicating whether to allow the API call.
4. If allowed, the API call is proxied to the Node.js API implementation.
5. The API response is returned from the API implementation.
6. (Optional) Metadata is sent to Edge for centralized analytics and monitoring.


### Running the API on Apigee Edge

You can deploy and run the entire API implementation on the Apigee Edge platform.

![Alt text](https://raw.githubusercontent.com/apigee-127/a127-documentation/master/a127/images/on-edge.png)

1. An app makes an API call to Apigee Edge.
2. Volos leverages direct access to Apigee Edge Services.
3. The API call is processed using the normal Apigee Edge flow.

### Running the API independently of Apigee Edge

TBD.