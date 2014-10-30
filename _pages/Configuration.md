---
layout: default
title: Config
permalink: /config/
---

# Configuring Apigee-127

The Apigee-127 configuration system provides a powerful, flexible way to configure projects for whatever deployment use case you can imagine.

Through a set of hierarchical configuration files, and a strict precedence order for loading and resolving them, you can inject individual values **and** complex objects into your project's Swagger file at the time you start or deploy your project. You can also create account-specific configurations, giving you added flexibility depending on where you wish to deploy your project.

* [About Apigee-127 configuration](#about)
* [How configuration works](#how)
* [Location of configuration files](#location)
* [Configuration file precedence](#precedence)
* [Setting config values in your account](#settingvalues)
* [More examples](#examples)

## <a name="about"></a>About Apigee-127 configuration

Using the anchor/alias feature supported by YAML, Apigee-127 can reference and replace config values (and complex objects) in the Swagger file when it is loaded.

In the following diagram, a single value from a config file is injected into the Swagger file. Note that you can [replace complex objects](#replacedec) as well as single values.

![alt text](https://raw.githubusercontent.com/apigee-127/a127-documentation/master/a127/images/config.png)

For example, if the following key/value pairs are added to one of the [supported config files](#location):

       apigeeProxyKey: 7nXWZAbque9n38IOdG2ZIbET1
       apigeeProxyUri: 'http://jdoe-test.apigee.net/apigee-remote-proxy'

...you can configure corresponding "anchors" (denoted by `&`) in the Swagger file through the `x-a127-config` vendor extension. Note that elements `apigeeProxyKey` and `apigeeProxyUri` exactly match the key names from the config file:

      x-a127-config:
          apigeeProxyKey: &apigeeProxyKeyAnchor CONFIGURED   # default value
          apigeeProxyUri: &apigeeProxyUriAnchor CONFIGURED   # default value

...you can reference those anchors anywhere in the Swagger file with aliases (denoted by `*`), like this:

    oauth2:
        provider: volos-oauth-apigee
        options:
          key: *apigeeProxyKeyAnchor
          uri: *apigeeProxyUriAnchor

...effectively, after the substitutions are made the result is this:

      oauth2:
        provider: volos-oauth-apigee
        options:
          key: 7nXWZAbque9n38IOdG2ZIbET1
          uri: 'http://jdoe-test.apigee.net/apigee-remote-proxy'

You can do a lot more than replace simple values with the Apigee-127 configuration system. This topic discusses other use cases and examples. In effect, almost everything in an Apigee-127 project is configurable!

## <a name="how"></a>How configuration works

Apigee-127 Swagger files are [YAML](http://www.yaml.org/) documents. YAML has the ability to use anchors and aliases that are resolved at load-time.

>Note: From the YAML specification -- An anchor is denoted by the “&” indicator. It marks a node for future reference. An alias node (denoted by "*") can then be used to indicate additional inclusions of the anchored node.

This YAML feature allows Apigee-127 to make substitutions in the Swagger file before the YAML resolution happens. Thus YAML, in combination with the configuration system of Apigee-127, gives you the ability to resolve or replace configured elements at load-time throughout the Swagger document.

## <a name="location"></a>Location of configuration files

You have the option to store configuration key/value pairs in three locations within an Apigee-127 project:

* [Apigee-127 account](#secrets)
* [config/default.yaml](#default)
* [config/[ACCOUNT_NAME].yaml](#name)

The key/value pairs are read by Apigee-127 and used to replace anchors in the Swagger file at load-time. They are loaded by a specific [order of precedence](#precedence).

### <a name="secrets"></a>Apigee-127 account

You can add key/value pairs to an Apigee-127 account with the `a127 account setValue key value` command (see [Setting config values in your account](#settingvalues) for details). When you start or deploy a project, the values configured for the currently active account are processed and substituted in corresponding anchors in the Swagger file. For example, if this key/value appears in your account:

    foo:bar

It can be referenced in Swagger in the `x-a127-config` extension like this:

     x-a127-config:
        foo: &fooAnchor DEFAULT-VALUE

    options:
        somekey: *fooAnchor

At load-time, the "somekey" element receives the value "bar".

### <a name="default"></a>config/default.yaml

The `default.yaml` file provides another option for storing config keys/values that are loaded into Swagger at load-time. The default `default.yaml` config is always loaded no matter which account is active. This option may allow flexibility in specifying certain default values, for example.

> Note: While you can only store key/value pairs (strings or numbers) in your Apigee-127 account, you can store entire complex objects in this YAML file, and they will be substituted in the Swagger file when it is loaded. For an example, see [Example 3: Replacing an entire declaration](#replacedec).

### <a name="name"></a>config/[ACCOUNT_NAME].yaml

For yet another option, you can store config in `config/[ACCOUNT_NAME].yaml`. In this case, you can store configuration values that are account-specific. If you're running your project under an account named 'apigee', then the `apigee.yaml` config file will be loaded. This config file gives you another opportunity to design your configuration with maximum flexibility.

> Note: While you can only store key/value pairs (strings or numbers) in your Apigee-127 account, you can store entire complex objects in this YAML file, and they will be substituted in the Swagger file when it is loaded. For an example, see [Example 3: Replacing an entire declaration](#replacedec).

## <a name="precedence"></a>Configuration file precedence

During application start or deploy, configuration is loaded and resolved according to specificity as follows:

`config/default.yaml < config/[NAME].yaml < active Apigee-127 account`

Configuration elements on the right override elements with the same name to their left.

So, for example, if a value for the key `myConfigElement` is specified in both `default.yaml` and in the active Apigee-127 account, the account value takes precedence -- it will be the replacement value used in the Swagger file.

## <a name="settingvalues"></a>Setting config values in your account

You can set key/value pairs in your currently active account using the `a127 account setValue key value` command.

Example:

    a127 account setValue myConfigElement foo

This command adds the key value pair to your active account:

    Account
    =======
    organization: jdoe
    username: jdoe@example.com
    password: '******'
    environment: test
    apigeeProxyKey: 7nXWZAbque9n38IoDfxdG2ZIbET1
    apigeeProxyUri: 'http://jdoe-test.apigee.net/apigee-remote-proxy'
    baseuri: 'https://api.enterprise.apigee.com'
    virtualhosts: 'default,secure'
    provider: apigee
    myConfigElement: foo

Now, the key/value pair can be referenced in the Swagger document at load-time.

## <a name="examples"></a>More examples

Here are several examples demonstrating the use and flexibility of Apigee-127 configuration.

### Example 1: Replacing a provider

The Volos.js provider is replaced by the "cacheProvider" key in the config file when the project is started:

    x-a127-config:
      cacheProvider: &cacheProvider volos-cache-memory  # replaced by "cacheProvider" key in config
    x-volos-resources:
      cache:
        provider: *cacheProvider

In this case, note that the `volos-cache-memory` provider is the default. Be sure to include any potential modules required by the resolved provider in the project's `package.json`.

### Example 2: Replacing a controller

Here, the controller is set based on a configured value:

    x-a127-config:
      twitterController: &twitterController twitter # replaced by "twitterController" in config
    paths:
      /twitter:
        x-swagger-router-controller: *twitterController


### <a name="replacedec"></a>Example 3: Replacing an entire declaration

The `x-a127-config` extension also allows for complex objects in the configuration file to be "aliased" in the Swagger file.

Let's say the configuration file `default.yaml` includes a complex declaration like this:

    oauth:
        provider: volos-oauth-apigee
        options:
        key: xyz123
        uri: 'http://example.com'

Then, you can reference this complex declaration in the Swagger file, in the same way you reference specific values:

    x-a127-config:
      oauth: &volosOAuth        # replaced by "oauth" key in config
        provider: volos-oauth-apigee
        options:
          key: VNE6pxpoHxR3PeGCAb8tUk05TTg5R
          uri: 'http://my-test.apigee.net/apigee-remote-proxy'
    x-volos-resources:
      oauth: *oauth

In this case, the entire "oauth" object from the config file is substituted into the Swagger. After resolution, you'd effectively have this configuration:

      x-volos-resources:
        oauth:
            provider: volos-oauth-apigee
            options:
                key: xyz123
                uri: 'http://example.com'







