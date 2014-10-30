---
layout: default
title: About
permalink: /about/
---

# Adding Apigee Edge Analytics to your API

* Add `volos-analytics-apigee` to your dependencies:
* Execute: `$ npm install `
* Open up the swagger editor by running `a127 project edit`  and add analytics to `x-volos-resources`:

  ```yaml
  analytics:
    provider: "volos-analytics-apigee"
    options:
      key: *apigeeProxyKey
      uri: *apigeeProxyUri
      proxy: WeatherExample,
      bufferSize: 100,
      flushInterval: 10,
      batchSize: 10
  ```
  The options that can be passed in are apigeeAnalytics object are:
   * **bufferSize**: Max number of records to be stored in memory.
   * **proxy**: Proxy to be associated with the in the analytics UI.
   * **flushInterval**: Intervals at which records are uploaded to Apigee, in miliseconds.
   * **batchSize**: Number of analytics records sent to Apigee in each batch.

* Finally, add analytics to each path you want analytics for:
  ```yaml
  /weather:
    x-swagger-router-controller: weather
    x-volos-authorizations:
      oauth2: {}
    x-volos-apply:
      analytics: {}
  ```
* Run the application as before, by executing `a127 project start`. Test the program using `curl`:

  ```bash
  $ curl http://127.0.0.1:10010/weather\?city\=San%20Jose,CA
  ```

  Run the command at least ten times. Then you can verify that the analytics shows up on the Apigee UI by looking at API Proxy performance under the Analytics menu.

You can also add [caching](https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Add-Caching), [quotas](https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Add-Quota), [OAuth](https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Add-Apigee-OAuth), and [deploy to Apigee](https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Deploy-To-Apigee-Edge).