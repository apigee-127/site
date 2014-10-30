---
layout: default
title: Understanding volos
permalink: /understanding-volos/
---

# What is Volos.js?

Volos.js is a set of [Connect](https://github.com/senchalabs/connect#readme)-compatible services written in Node.js that make it possible for developers to easily add common API design patterns like security and traffic management to their code.

# Volos.js features

Volos.js includes Node.js modules for adding these services to an API:

* **Caching:** Response caching that can be configured by URI or custom function.

* **Analytics:** Analytics that can published to [Apigee Edge Analytics](http://apigee.com/about/resources/videos/edge-analytics-dashboard-tour).

* **OAuth 2.0:** Full OAuth 2.0 Server or OAuth 2.0 proxy to Apigee Edge.

* **Quota:** Quota on a per-API, per-resource, per-header or per-parameter basis, or with a customized function.

# Execution environment options

Developers can choose to configure Volos.js to run in several different modes:

* **[Standalone mode](#running-the-api-independently-of-apigee-edge):** Volos.js supports a standalone mode that relies entirely on open-source components.

* **[Deploy anywhere, use Apigee Remote Services](#api-running-in-conjunction-with-apigee-edge):** Volos.js can run in any environment that supports Node.js and communicate with Apigee Edge using APIs, so that the full power of Apigee can be used to manage and configure developers, apps and APIs.

* **[Deployed on Apigee Edge](#api-deployed-on-apigee-edge):** When Volos.js applications are deployed to the Apigee platform, they will take advantage of local optimizations so that they run with the best possible availability and performance. They can also add optimizations such as detailed API analytics.


> **Note:** If you run Volos.js on Apigee Edge or in conjunction with Apigee Edge, you must have an Apigee account and deploy a the [Apigee Remote Proxy](https://github.com/apigee-127/volos/tree/master/proxy) to Edge.  If you use Apigee-127 this is handled for you when you create an account configuration with Apigee as the provider.


## <a name='running-the-api-independently-of-apigee-edge'></a>Standalone: Running the API independently of Apigee Edge

You can run your API anywhere and never have a dependency on Apigee Edge.  Volos.js has libraries that enable you to use either memory or Redis for Caching, tracking Quotas and managing OAuth 2.0 Tokens.  A simple configuration lets you chose between the two.

Volos.js is an open source project that uses a [Service Provider Interface](http://en.wikipedia.org/wiki/Service_provider_interface) model to abstract Services such as Caching and Quota from Providers such as Apigee, in-memory and Redis.  If you want to use a different provider you can fork the repository and give it a try yourself or open an [Issue](https://github.com/apigee-127/volos/issues) for us to take a look at.

## <a name='api-running-in-conjunction-with-apigee-edge'></a>Deployed anywhere and running in conjunction with Apigee Edge

Running locally or deployed to any Node.js environment of your choice, your API make calls to Apigee Edge to consume Services like quota management, OAuth and Analytics.  Volos provides the glue that binds together your API implementation and Apigee Edge.

![Alt text](https://raw.githubusercontent.com/apigee-127/a127-documentation/master/a127/images/with-edge.png)

1. Apps make API calls to the Node.js endpoint which uses Volos.js libraries.
2. Volos.js calls the Apigee Remote Proxy in your Organization on Apigee Edge to consume the configured services such as OAuth or Quota and to send Analytics data. Volos.js only sends metadata to Edge, not API payloads, so you can feel comfortable with the security of your API payloads never leaving your network.
3. For Quota and/or OAuth Edge returns a response indicating whether to allow the API call - either due to Quota or OAuth token validation.
4. If allowed, the API call is proxied to the Node.js API implementation.
5. The API response is returned from the API implementation.
6. (Optional) Metadata is sent to Edge for centralized analytics and monitoring

### <a name='api-deployed-on-apigee-edge'></a>API deployed on Apigee Edge

You can deploy and run the entire API implementation on the Apigee Edge platform and take advantage of the full suite of API Management features on Edge.  The Apigee Access module enables your Node.js API to consume Apigee services directly for Caching, Quota and OAuth with no code changes (really)!

![Alt text](https://raw.githubusercontent.com/apigee-127/a127-documentation/master/a127/images/on-edge.png)

1. An app makes an API call to Apigee Edge.
2. Volos leverages direct access to Apigee Edge Services.
3. The API call is processed using the normal Apigee Edge flow.


# Further Reading

* Volos.js on [GitHub](https://github.com/apigee-127/volos)
* Examples link
* Getting started [training sample](https://github.com/apigee-127/volos/blob/master/samples/training/README.md)