---
layout: default
title: About
permalink: /about/
---

# Quick start: Add caching to your API

This topic explains how to add caching to the quick start API using Volos.js Swagger extensions.

* [Before you begin](#before)
* [Configuring the cache in the Swagger editor](#configure)
* [Testing the cache configuration](#test)
* [Using a helper function to customize caching](#custom)


## <a name='before'></a>Before you begin

If you're using the default skeleton project (the one that's installed when you do `a127 project create`), then you can skip this step.

But just keep in mind that you must include one of the `volos-cache-*` modules in your project's `package.json` file. In this example, we're going to use `volos-cache-memory`. If you change `package.json`, remember to run `npm install` in the project root folder to install the dependencies.

For information on the `volos-cache-*` modules, see [the Volos.js page on GitHub](https://github.com/apigee-127/volos).

## <a name='configure'></a>Configuring the cache in the Swagger editor

Adding a cache using Volos.js is as simple as adding a few lines of YAML in your API's Swagger specification file.

1. Execute `$ a127 project edit` to open up the Swagger editor.
2. In the Swagger configuration file, under `x-volos-resources`, add the following to enable an in-memory cache provider named `cache`. In this case, the cache is set with a TTL (time to live) of 60000 milliseconds.

  ```yaml
    x-volos-resources
      cache:
        provider: volos-cache-memory
        options:
          name: hello-cache
          ttl: 60000
  ```

3. Finally, use the `x-volos-apply` extension to apply this policy to the paths you want to enforce caching on:
```yaml
     paths:
       /hello:
          x-swagger-router-controller: hello_world
          x-volos-authorizations: {}
          x-volos-apply:
            cache: {}
            quota: {}
```

## <a name='test'></a>Testing the cache configuration

You set up your cache to hold values in memory for 60000 milliseconds. Let's test it out.

1. Start the app with `a127 project start`.
2. Call the API in succession:
  ```bash
          $ curl http://localhost:10010/hello\?name\=Me
          $ curl http://localhost:10010/hello\?name\=Me
          $ curl http://localhost:10010/hello\?name\=Me
  ```

The caching policy caches responses for the specified TTL duration.

## <a name='custom'></a>Using a helper function to customize caching

If you do not specify a helper function to evaluate the Cache Key the entire URI will be used.  This is less effective if you have API keys or other query parameters that result in many distinct URIs that could be cached using the same key.

The example above shows how you can use a single query parameter, `city` in this case, as the Cache Key

```yaml
paths:
    /hello_cached:
        x-swagger-router-controller: hello_world
        x-volos-apply:
          cache:
            key:
              helper: volos
              function: cacheKey
        get:
          ...
```

Using this annotation instructs Volos.js to call the function `cacheKey` in the helper module `volos` which it will look for in `api/helpers` by default.  Here is an example implementation that uses the `name` query parameter to evaluate the Cache key:

```javascript
module.exports = {
  cacheKey: cacheKey,
};

function cacheKey(req) {
// This can check for a specific query parameter
  var key = req.swagger.params.name.value;
  console.log('Cache Key: '+key)
  return key;
}
```

> Tip: You can add whatever logic you wish to use to evaluate the Cache key, but don't add too much complexity or your latency may go way up!

