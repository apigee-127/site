---
layout: default
title: Spike
permalink: /spike/
---

# Quick start: Add a spike arrest policy to the API

A Spike Arrest policy protects against traffic spikes. It throttles the number of requests processed by an API proxy and sent to a backend, protecting against performance lags and downtime.

This topic explains how to add a spike arrest policy to an Apigee-127 API.

* [Before you begin](#before)
* [Configuring spike arrest in the Swagger editor](#configure)
* [Testing the spike arrest configuration](#test)
* [Spike arrest options](#options)
* [How it works](#howitworks)
* [Using a helper function for customized spike arrests](#spikearrest-helper)
* [Related information](#related)

## <a name='before'></a>Before you begin

If you're using the default skeleton project (the one that's installed when you do `a127 project create`), then you can skip this step.

But just keep in mind that you must include one of the `volos-spikearrest-*` modules in your project's `package.json` file. In this example, we're going to use `volos-spikearrest-memory`. If you change `package.json`, remember to run `npm install` in the project root folder to install the dependencies.

For information on the `volos-spikearrest-*` modules, see [the Volos.js page on GitHub](https://github.com/apigee-127/volos).

## <a name='configure'></a>Configuring spike arrest in the Swagger editor

Adding a spike arrest policy using Volos.js is as simple as adding a few lines of YAML in your API's Swagger specification file.

1. Execute `$ a127 project edit` to open up the Swagger editor.
2. In the Swagger configuration file, under `x-volos-resources`, add the following to enable an in-memory spike arrest provider. In this case, the spike arrest limit is set to allow no more than 10 API calls per minute. See the section "[How it works](#howitworks)" for more details.

  ```yaml
    x-volos-resources:
      spikeArrest:
        provider: "volos-spikearrest-memory"
        options:
          timeUnit: "minute"
          bufferSize: 10
          allow: 10
  ```

3. Finally, use the `x-volos-apply` extension to apply this policy to the paths you want to enforce spike arrest on:

  ```yaml
       paths:
         /hello:
            x-swagger-router-controller: hello_world
            x-volos-authorizations: {}
            x-volos-apply:
              spikearrest: {}
  ```

## <a name='test'></a>Testing the spike arrest configuration

You set up your spike arrest to allow 10 API calls per minute. When this limit is exceeded, the API returns an error. Let's test it out.

1. Start the app with `a127 project start`.
2. Call the API several times in quick succession:
  ```bash
          $ curl http://localhost:10010/hello\?name\=Me
           ...
            Error: Error: SpikeArrest engaged<br> &nbsp; &nbsp;at...
  ```

The spike arrest policy is enforced if the number of calls violates the specified rule. The next section explains in more detail how the spike arrest policy works.

## <a name='options'></a>Spike arrest options

```yaml
    options:
      timeUnit: "minute"
      bufferSize: 10
      allow: 10
```

* **timeUnit:** How often the SpikeArrest resets - may be in seconds or minutes.

* **allow:** The maximum number of requests to allow during the timeUnit.

* **bufferSize:** (optional, default = 0) if bufferSize > 0, SpikeArrest will attempt to smooth requests by returning only when the next appropriate execution window is available. bufferSize is how many requests to "queue" before returning (immediately).

## <a name='howitworks'></a>How it works

Think of Spike Arrest as a way to generally protect against traffic spikes rather than as a way to limit traffic to a specific number of requests. Your APIs and backend can handle a certain amount of traffic, and the Spike Arrest policy helps you smooth traffic to the general amounts you want.

To prevent spike-like behavior, Spike Arrest calculates the allowed traffic into windows by dividing your settings into appropriate intervals:

* Per-minute rates get smoothed into requests allowed in intervals of seconds:

    For example, 30 per minute gets smoothed like this: 60 seconds (1 minute) / 30pm = 2-second intervals, or about 1 request allowed every 2 seconds. A second request inside of 2 seconds will fail if there is no buffer. Also, a 31st request within a minute will fail.

* Per-second rates get smoothed into requests allowed in intervals of milliseconds.

    For example, 10 per second gets smoothed like this: 1000 milliseconds (1 second) / 10ps = 100-millisecond intervals, or about 1 request allowed every 100 milliseconds. A second request inside of 100ms will fail if there is no buffer. Also, an 11th request within a second will fail.

If the Spike Arrest is set up with a buffer (bufferSize > 0), however, it will not fail immediately when too many requests are made concurrently. Instead, SpikeArrest will attempt to smooth requests by returning success to the caller once the next appropriate execution window is available. Once the buffer is full, it will fail all further requests until a new execution window becomes available.

Using a buffer can be a great way to continue to fulfill requests during an quick, odd spike. However, be aware that when requests exceed the SpikeArrest window, this will delay responses up to: bufferSize * (timeUnit / allow).

<<<<<<< HEAD
Per-minute rates get smoothed into requests allowed in intervals of seconds.
For example, 30pm gets smoothed like this:
60 seconds (1 minute) / 30pm = 2-second intervals, or about 1 request allowed every 2 seconds. A second request inside of 2 seconds will fail. Also, a 31st request within a minute will fail.
Per-second rates get smoothed into requests allowed in intervals of milliseconds.
=======
For example, 10 per second with a buffer of 10 means requests caught in the queue could be delayed up to 1 second!
>>>>>>> 1ba1e91ab6d9b4bf43f707e97f1c4f444ad193d5


## <a name='spikearrest-helper'></a>Using a helper function for customized spike arrests

Coming soon.

## <a name='related'></a>Related information

See the [volos-spikearrest-common](https://github.com/apigee-127/volos/tree/master/spikearrest/common) description on GitHub for details on the underlying Volos.js module.
