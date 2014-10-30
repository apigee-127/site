---
layout: default
title: Adding OAuth 2.0
permalink: /addiing-oauth/
---


# Adding OAuth 2.0 security to your API

[Work in progress...]

This topic explains how to add OAuth 2.0 security to your Apigee-127 API using [Volos.js](https://github.com/apigee-127/a127-documentation/wiki/Understanding-Volos.js) configurations placed in the API's Swagger specification file.

* Examples and code samples
* About Volos.js support for OAuth 2.0
* Overview of steps

## Examples and code samples

See how to add OAuth 2.0 security to the "quick start" API with Apigee as the token manager. See [Quick Start: Add Apigee OAuth 2.0 to the API](https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Add-Apigee-OAuth) for an end-to-end, working example.

The [weather-advanced](https://github.com/apigee-127/a127-samples/tree/master/weather-advanced) sample project on GitHub shows how to add OAuth 2.0 to an API using Apigee Edge as the token manager.

The [example-project](https://github.com/apigee-127/example-project) sample on GitHub demonstrates how to use the Volos.js API to create and manage OAuth tokens programmatically.

You can find an example[here](https://github.com/apigee-127/volos/tree/master/oauth/common) that shows how to configure OAuth 2.0 using Redis as the store for token management.

## About Volos.js support for OAuth 2.0

OAuth 2.0 is provided to Apigee-127 through the [Volos.js](https://github.com/apigee-127/a127-documentation/wiki/Understanding-Volos.js) project. Volos.js supports two Node.js implementations for adding OAuth 2.0 security to an API:

* [volos-oauth-redis](https://www.npmjs.org/package/volos-oauth-redis): Uses Redis as a database for managing OAuth tokens and related activities.

* [volos-oauth-apigee](https://www.npmjs.org/package/volos-oauth-apigee): Communicates with Apigee Edge through APIs for managing OAuth tokens and related activities.

## Requesting an access token using the client credentials grant type

This technique is covered in the following topic and sample project:

See [Quick Start: Add Apigee OAuth 2.0 to the API](https://github.com/apigee-127/a127-documentation/wiki/Quick-Start:-Add-Apigee-OAuth) for an end-to-end, working example.

For another example, see the [weather-advanced](https://github.com/apigee-127/a127-samples/tree/master/weather-advanced) sample project on GitHub.

### Requesting an access token using the password credentials grant type

If you want to use password credentials, you need a way to validate user credentials before retrieving an access token from Apigee. Here's how:

1. Add the `passwordCheck` stanza in the Swagger spec in the oauth2 section of the `x-volos-resources` extension.

>Note: The `passwordCheck` stanza specifies in which "helper" file the app can find the passwordCheck() method. In this case, it looks in `api/helpers/volos.js`. The uncommented stanza should look like this:

  ```yaml
      x-volos-resources:
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
          passwordCheck:
            helper: volos
            function: passwordCheck
          tokenPaths:  # These will be added to your paths section for you
            authorize: /authorize
            token: /accesstoken
            invalidate: /invalidate
            refresh: /refresh
  ```

2. Implement the `passwordCheck` function in `api/helpers/volos.js`.

The sample code below simply satisfies the minimal requirement of having a passwordCheck() method. You can copy and paste it into `api/helpers/volos.js`.

>Note: In reality, you might implement this method to verify the user's credentials in an LDAP system, for example.

  ```javascript
  module.exports.passwordCheck = passwordCheck;

  function passwordCheck(username, password, cb) {
    // Implement as necessary
    var passwordOk = (username === 'scott' && password === 'apigee');
    cb(null, passwordOk);
  }
  ```

3. Start the app (`a127 project start`).
4. Obtain consumer keys from Apigee Edge. You'll need to plug them in the next step. See [Obtaining consumer keys from Apigee Edge](#obtainkeys) below.
4. Obtain the access token by calling this API. Fill in the `client_id` and `client_secret` values with the consumer keys obtained from Apigee Edge. The username and password must be validated by the passwordCheck() method in `api/helper/volos.js`.

  `curl -X POST "http://localhost:10010/accesstoken" -d "grant_type=password&client_id=my-client-id&client_secret=my-client-secret&password=apigee&username=scott"`

This method returns an access token:

  ```json
  {
      "access_token": "7zSVVqNCsGYQKWDKy2C02XGOTUBA",
      ...
  ```

Now, with an access token, you can execute the API by calling it like this:

`curl -i "http://localhost:10010/hello?name=Scott" -H "Authorization: Bearer 7zSVVqNCsGYQKWDKy2C02XGOTUBA"`

## <a name="obtainkeys">Obtaining consumer keys from Apigee Edge

Here is the procedure for obtaining the keys from Apigee Edge. Briefly, you need to create a Developer App on Edge -- that App will include the keys you need.

>Note: If you already have a Developer App on Edge that you can use, then you can skip these the first three steps below.

1. Sign in to your [Apigee account](http://www.apigee.com).
1. Create a new Product. (**Publish > Product** and fill in the form. You can leave the API Proxy field blank -- it isn't necessary to select an API proxy for this exercise.)
2. Create a new Developer. (**Publish > Developer** and fill out the form).
3. Create a new Developer App. (**Publish > Developer App** and fill out the form, and click **Save**)
4. Open the Developer App you just created. Click **Show** next to the Consumer Key and Consumer Secret fields (in the following figure, the keys are shown):

![alt text](https://raw.githubusercontent.com/apigee-127/a127-documentation/master/a127/images/dev-app.png)


## <a name="obtainkeysvolos">Obtaining the access token programmatically

You can find a working example where the Access token is fetched from Apigee Edge programmatically using Volos.js. The relevant code is in app.js in the [example-project](https://github.com/apigee-127/example-project) on GitHub.


