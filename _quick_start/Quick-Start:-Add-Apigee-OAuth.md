---
layout: default
title: About
permalink: /about/
quick_start: true
---

# Quick Start: Add Apigee OAuth 2.0 to the API

This topic explains how to add OAuth 2.0 support to the "quick start" example API. In this example, we'll configure the API to use the Apigee Edge OAuth 2.0 token manager to obtain access tokens.

>Note: For a more detailed discussion of adding OAuth 2.0 support, see [Adding OAuth 2.0](https://github.com/apigee-127/a127-documentation/wiki/Adding-OAuth).

* [Before you begin](#beforeyoubegin)
* [Configuring OAuth 2.0 for your project](#configuring)
* [Verifying the configuration](#verifyconfig)
* [Obtaining an access token](obtainaccesstoken)
* [Calling the secure API](callapi)
* [Summary](summary)

## <a name="beforeyoubegin"></a>Before you begin

To use Apigee Edge as the OAuth 2.0 token manager, you need to create an Apigee-127 account with your Apigee account information. If you already created an Apigee-127 account, you can skip these steps. To verify that you have an active account, execute `a127 account list`.

1. Execute `a127 account create <account-name>`.
2. Follow the prompts to create an Apigee-127 account using Apigee as the account provider.

>Note: Be sure to select `apigee` as the provider.

 `a127 account create my-account`
```bash
    [?] Provider?
      amazon
    ❯ apigee
      local
    ...
```

>Note: If you do not have an account on Apigee Edge, you'll be prompted to create one.

For more details on account creation, see the [Apigee-127 Command-Line Reference](https://github.com/apigee-127/a127-documentation/wiki/Apigee-127-command-line-reference).

## <a name="configuring"></a>Configuring OAuth 2.0 for your project

Now, let's add OAuth security to the API. Most of the configuration work is done right in the Swagger editor with [Volos.js](https://github.com/apigee-127/a127-documentation/wiki/Understanding-Volos.js) extensions.

1. Create and test the "quick start" sample API, as explained in "[Quick start](https://github.com/apigee-127/a127-documentation/wiki/Quick-start).
2. Be sure you have created an Apigee-127 account with your Apigee account information, as explained in the previous section.

   Note: Make sure that your project's `package.json` file includes `volos-oauth-apigee` module in the dependencies list. If you're using the default skeleton project, you don't need to do anything -- the module is already included in `package.json`.

2. Open `app.js` and uncomment `app.use(express.bodyParser());`.
3. Open the Swagger editor: `a127 project edit`.
4. Look for these lines:

  ```yaml
    x-a127-config: {}
    x-volos-resources: {}
  ```

5. Replace those lines with the following stanzas.

    **Important:** Add these stanzas exactly as shown. These are top-level tags -- the extension names should be intented all the way to the left.

  ```yaml
  x-a127-config:
    apigeeProxyKey: &apigeeProxyKey CONFIGURED
    apigeeProxyUri: &apigeeProxyUri CONFIGURED
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
        tokenPaths:
          authorize: /authorize
          token: /accessToken
          invalidate: /invalidate
          refresh: /refresh
  ```
6. Add `x-www-form-urlencoded` to the "consumes" part of the spec, like this:

  ```yaml
    consumes:
      - application/json
      - application/x-www-form-urlencoded
  ```

7. Apply an `oauth2` authorization to the `/hello` path as follows:

  ```yaml
    /hello:
      x-swagger-router-controller: hello
      x-volos-authorizations:
        oauth2: {}
  ```


That's it! You've added OAuth 2.0 security to the API. Now, you need to go through a few steps to obtain an access token and test your API. We'll walk through these steps next.

## <a name="verifyconfig"></a>Verifying the configuration

To test the OAuth configuration, you need to obtain an access token (also called a bearer token) from the Apigee Edge OAuth manager.

First, verify that the security mechanism is working. Start up the app and try calling it. For example,

1. `a127 project start`

2. `curl -i http://localhost:10010/hello?name=Scott`

    ```bash
        {"error_description":"Missing Authorization header","error":"missing_authorization"}
    ```

If you added OAuth correctly, the request is rejected because you need to obtain and present an access token.

>Tip: If you get a message like Cannot POST /accesstoken, be sure that you uncommented app.use(express.bodyParser()) in app.js.

## <a name="obtainaccesstoken"></a>Obtaining an access token

Let's get an access token using the OAuth 2.0 Client Credentials grant type.

>Note: OAuth 2.0 is a fairly complex topic. You can find many articles on the web. If you're new to OAuth 2.0, we recommend that you familiarize yourself with concepts like the client credentials grant type.

You can request an access token by calling this API.

>Note: The route for this API is `/accesstoken`, which you added as one of the `tokenPaths` in the Swagger file.

`curl -X POST "http://localhost:10010/accesstoken" -d "grant_type=client_credentials&client_id=<my-client-id>&client_secret=<my-client-secret"`

Obviously, there's a missing step here. Before you can call this API, you need to obtain values for the `client_id` and `client_secret` parameters.

For this example, we'll show you how to obtain those values directly from Apigee Edge through the Edge management UI.

>Note: You can also obtain the values programmatically. For details, see [Adding OAuth 2.0](https://github.com/apigee-127/a127-documentation/wiki/Adding-OAuth).

Here is the procedure for obtaining the keys from Apigee Edge. Briefly, you need to create a Developer App on Edge -- that App will include the keys you need.

>Note: If you already have a Developer App on Edge that you can use, then you can skip these the first three steps below.

1. Sign in to your [Apigee account](http://www.apigee.com).
1. Create a new Product. (**Publish > Product** and fill in the form. You can leave the API Proxy field blank -- it isn't necessary to select an API proxy for this exercise.)
2. Create a new Developer. (**Publish > Developer** and fill out the form).
3. Create a new Developer App. (**Publish > Developer App** and fill out the form, and click **Save**)
4. Open the Developer App you just created. Click **Show** next to the Consumer Key and Consumer Secret fields (in the following figure, the keys are shown):

![alt text](https://raw.githubusercontent.com/apigee-127/a127-documentation/master/a127/images/dev-app.png)

## <a name="callapi"></a>Call the secure API

To call your secure API, substitute the Consumer Key and Consumer Secret values for the `client_id` and `client_secret` attributes in the `/accesstoken` API call, as follows:

>Note: Here, we pipe the output to a Python utility that prints JSON in a nice, indented format. You can freely obtain this utility if you wish.

  ```bash
    curl -X POST "http://localhost:10010/accesstoken" -d "grant_type=client_credentials&client_id=o2lxnymziDkjXNgGtb9&client_secret=S9vUARV" | python -m json.tool
    {
        "access_token": "7zSVVqNCsGYQKWDKyXGOTUBA",
        "api_product_list": "[Test App product]",
        "api_profile_name": null,
        "application_name": "921c6d-78c0-4b81-9884-c1f7d7b8f",
        "client_id": "o2lxnymzY9iDkjXb9PsNJCZNJXVP",
        "developer_email": "someperson@example.com",
        "expires_in": 1799,
        "issued_at": 1411398701495,
        "organization_id": "0",
        "organization_name": "wwitman",
        "refresh_count": 0,
        "refresh_token": null,
        "refresh_token_expires_in": 0,
        "refresh_token_status": null,
        "scope": "",
        "state": null,
        "status": "approved",
        "token_type": "bearer"
    }
  ```

Now, you have an access token and you can execute the `/hello` API by calling it like this:

>Note: Be sure to start the project before calling the API: `a127 project start`):

`curl -i "http://localhost:10010/hello?name=Scott" -H "Authorization: Bearer 7zSVVqNCsGYQKWDKyXGOTUBA"`

And it returns:

`Hello, Scott`

If this succeeds, it shows that the access token was accepted, and you've successfully completed the OAuth 2.0 config.

## <a name="summary"></a>Summary

If you've followed these steps, you have added OAuth 2.0 security to the "quick start" API. With this security in place, no one can call the API without a valid access token.

Follow the same pattern to add OAuth support (with Apigee as the manager) for any other API. For a more detailed dive into OAuth support, see [Adding OAuth 2.0](https://github.com/apigee-127/a127-documentation/wiki/Adding-OAuth). In that topic, you'll learn how to add OAuth support that does not rely on Apigee (it uses Redis as the token manager) and how to implement the password grant type.