---
layout: default
title: On Perm
permalink: /on-perm/
---

# Using Apigee-127 with Apigee Edge on-premises

If you are using an on-premise installation of Apigee Edge, you need to configure your Apigee-127 account to point to the base URI for your on-premise Apigee Edge installation. You can then deploy APIs created with Apigee-127 to your on-prem installation.

Simply specify the base URI when you create an Apigee-127 account using the `a127 account create` command. For example:

````
    $ a127 account create myOnPremAccount
    [?] Provider? apigee
    [?] Do you have an account? Yes
    **[?] Base URI? https://api.your.apigee.onprem.com**
    [?] Organization? myorg
    [?] User Id? jdoe@example.com
    [?] Password? ******
    [?] Environment? prod
    [?] Virtual Hosts? default,secure
````

For more information on account creation, see the "[Apigee-127 command-line reference](https://github.com/apigee-127/a127-documentation/wiki/Apigee-127-command-line-reference)".


