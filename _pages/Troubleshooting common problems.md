---
layout: default
title: Troubleshooting
permalink: /troubleshooting/
---

## Troubleshooting tips

This topic lists common problems that you may encounter when using Apigee-127 and their solutions. Issues discussed in this topic include:

* Incorrect or expired keys
* Use of bodyParser() method when parsing a request body
* Error stating that node_modules is too big when you deploy to Apigee Edge
* Problem running npm on Windows
* How to verify that Usergrid is running
* Installation errors
* Changing the Usergrid Cassandra port

###Incorrect or expired keys

A possible source of error is using incorrect or expired app keys in your configuration. Make sure, for example, that your Twitter credentials are current.

###Use of bodyParser() method when parsing a request body

If your app needs to parse a request body, be sure to place the bodyParser() before including the Apigee-127 middleware. Here is the recommended pattern:

```javascript
var a127 = require('a127-magic');
var express = require('express');
var app = express();
app.use(express.bodyParser()); // Call this before including the a127.middleware()
app.use(a127.middleware());
app.listen(process.env.PORT || 10010);
```

###Error stating that node_modules is too big when you deploy to Apigee Edge

If you receive an error like this, use the `-r` option with `a127 project deploy` command. When this flag is set, the deployment tool ([apigeetool](https://www.npmjs.org/package/apigeetool)) does not ZIP and upload the contents of node_modules; rather, it runs `npm` on Apigee Edge.

###Problem running npm on Windows

For some versions of npm on Windows will have problems on the `npm install` step of `a127 project create`.  They are related to a `debug` module on npm not being managed properly.  The following steps should resolve this issue:

1. In the project directory, execute the following commands:
  1. `npm install yamljs`
  2. `npm install debug`
  3. `npm install swagger-tools`

Now, when you run `a127 project start` your project should start successfully.

### How to verify that Usergrid is running

If you try to start Usergrid with `a127 usergrid start`, and Usergrid is already running, you'll see the message "Usergrid is running." In some cases, you may see this message when Usergrid is not running. To verify that Usergrid is running, hit localhost:8080. If you get a "Page Not Found" error, then Usergrid is not actually running. If this happens, try stopping and starting Usergrid like this:

    $ a127 usergrid stop
    $ a127 usergrid start

### Installation errors

If you install Apigee-127 using the `sudo` option (described [here](https://github.com/apigee-127/a127-documentation/wiki/Installation)) and LATER decide to SWITCH to using the non-`sudo` option (described [here](https://github.com/apigee-127/a127-documentation/wiki/npm-Configuration-for-Mac-and-Linux)), you may receive errors. This may be caused by the permission settings on the `~/.a127` directory.

In this case, we recommend that you either remove or rename this directory, or change ownership on it and try installing again.

You can also try clearing the npm cache:

    $ rm -rf ~/.npm
    $ npm cache clear

### Changing the Usergrid Cassandra port

If necessary, you can change the default Cassandra port for Usergrid. The default is 7000. To change it, edit the file: ~/.a127/usergrid/tmp/cassandra.yaml

### Installation error

If you see this error when you install Apigee-127 on Linux or Mac OS X based systems, you may need to use the --`unsafe-perm` option with npm. For details, see the [Installation instruction page](https://github.com/apigee-127/a127-documentation/wiki/Installation).

    Tell the author that this fails on your system:
    npm ERR!     node lib/commands/project/download_skeleton.js
