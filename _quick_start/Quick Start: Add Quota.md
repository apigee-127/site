---
layout: default
title: About
permalink: /about/
quick_start: true
---

# Quick start: Add a quota to the API

This topic explains how to add a quota to the quick start API. A quota specifies the number of request messages that an app is allowed to submit to an API over the course of a time unit of hours, minutes, or days.

* [Before you begin](#before)
* [Configuring the quota in the Swagger editor](#configure)
* [Testing the quota configuration](#test)
* [Using a helper function for customized quotas](#quota-helper)

## <a name='before'></a>Before you begin

If you're using the default skeleton project (the one that's installed when you do `a127 project create`), then you can skip this step.

But just keep in mind that you must include one of the `volos-quota-*` modules in your project's `package.json` file. In this example, we're going to use `volos-quota-memory`. If you change `package.json`, remember to run `npm install` in the project root folder to install the dependencies.

For information on the `volos-quota-*` modules, see [the Volos.js page on GitHub](https://github.com/apigee-127/volos).

## <a name='configure'></a>Configuring the quota in the Swagger editor

Adding a Quota using Volos.js is as simple as adding a few lines of YAML in your API's Swagger specification file.

1. Execute `$ a127 project edit` to open up the Swagger editor.
2. In the Swagger configuration file, under `x-volos-resources`, add the following to enable an in-memory quota provider named `quota`. In this case, the quota limit is set to allow no more than 2 API calls per minute.

  ```yaml
    quota:
      provider: volos-quota-memory
      options:
        timeUnit: minute
        interval: 1
        allow: 2
  ```

3. Finally, use the `x-volos-apply` extension to apply this policy to the paths you want to enforce quotas on:
```yaml
     paths:
       /hello:
          x-swagger-router-controller: hello_world
          x-volos-authorizations: {}
          x-volos-apply:
            quota: {}
```

## <a name='test'></a>Testing the quota configuration

You set up your quota to allow two API calls per minute. On the third call within a minute, the API will return an error. Let's test it out.

1. Start the app with `a127 project start`.
2. Call the API three times in quick succession:
  ```bash
          $ curl http://localhost:10010/hello\?name\=Me
          $ curl http://localhost:10010/hello\?name\=Me
          $ curl http://localhost:10010/hello\?name\=Me
            Error: exceeded quota<br> &nbsp; &nbsp;at...
  ```

The quota policy only allows the first two requests and then returns an error on the third request (if made within a minute of the first request).

## <a name='quota-helper'></a>Using a helper function for customized quotas

The example above shows how you can enforce a global quota for your API - but what if you want per-Developer, per-App or per-Token Quotas?  We've got you covered!  You can also use a Helper function to customize how a Quota is calculated.

Here is an example:

```yaml
paths:
    /weather_quota:
        x-swagger-router-controller: weather
        x-volos-apply:
          quota:
            key:
              helper: volos
              function: quotaHelper
        get:
          ...
```

This annotation instructs Volos.js to call the `quotaHelper` function of the helper module `volos`, which it will look for in `api/helpers` by default.  Here is an example implementation that grabs and returns the `name` query parameter. You could implement custom logic here that manages and enforces the quota.

```javascript
module.exports = {
  quotaHelper: quotaHelper
};

function quotaHelper(req) {
  var key = req.swagger.params.name.value;
  console.log('Quota Key: '+key)
  return key;
}

```

>Tip: You can customize this helper with any logic you wish to maintain your quota, but don't add too much complexity or your latency may go way up!

