---
layout: default
title: Installation
permalink: /installation/
---

# Prerequisites
* [Node.js](http://nodejs.org/download/) version 0.10.24 or higher installed on the machine where you want to run Apigee-127
* npm version 1.3 or higher

# Installation
You can install `apigee-127` either through npm or by cloning and linking the code from GitHub.  This document covers the installation details for installing from npm.

### Linux / Mac OS X, in a Terminal Window:

Apigee-127 includes a command-line interface that is most easily used when it is on the PATH of your environment.  npm allows you to install a module globally using the `-g` option.  Using this option with a default configuration will attempt to put the `a127` executable in a directory that needs root permissions such as `/usr/local/bin`.  You have two options:


1. Use `sudo` to execute the command, in which case you will need to add the `--unsafe-perm` option.   The `--unsafe-perm` option gives npm permission to create a folder in the `~/.a127` directory to store things like the skeleton files for a starter project.  This command would be:

	```bash
    $ sudo npm install -g apigee-127 --unsafe-perm
	```

2. Configure npm to use a directory that does not require root privileges by setting the npm 'prefix'.  These steps can be found here: [https://github.com/apigee-127/a127-documentation/wiki/npm-Configuration-for-Mac-and-Linux](https://github.com/apigee-127/a127-documentation/wiki/npm-Configuration-for-Mac-and-Linux).

	Once you have configured npm in this manner, open a Terminal window and type the following command:

	```bash
   	$ npm install -g apigee-127
	```

	This will put the libraries into `~/npm/lib/node_modules` and executables and `~/npm/bin` if you use `~/npm` for your prefix.  Note that you also need to configure your `PATH` appropriately, using the steps described here: [https://github.com/apigee-127/a127-documentation/wiki/npm-Configuration-for-Mac-and-Linux](https://github.com/apigee-127/a127-documentation/wiki/npm-Configuration-for-Mac-and-Linux).

>Note: If you have a problem, see the [Troubleshooting](https://github.com/apigee-127/a127-documentation/wiki/Troubleshooting-common-problems) page.


### Windows, from a Command Prompt


	npm install -g apigee-127


## Location of files

Apigee-127 places most of its files that it depends on at a global level in `~/.a127`.  This includes an `accounts` file that has details about the configured accounts and an optional `usergrid` directory if you choose to download and use Usergrid through the `a127` command line.

## Dependencies

For a list of dependencies and Node.js modules that are relevant to an Apigee-127 project, see [Apigee-127 modules](https://github.com/apigee-127/a127-documentation/wiki/Apigee-127-modules)

## Handling SHASUM check errors

If you see an error like this when you try to install Apigee-127:

	````
	46 error Error: shasum check failed for /tmp/npm-9458-zywjMau6/registry.npmjs.org/apigee-127/-/apigee-127-0.6.2.tgz
	46 error Expected: 2c3bd0b773d5889886d561ec84ff97b23f2a1b2b
	46 error Actual:   7e5e107ea02823774272641a2b50ee70e17eccf3
	46 error From:     https://registry.npmjs.org/apigee-127/-/apigee-127-0.6.2.tgz
	46 error     at /usr/lib/node_modules/npm/node_modules/sha/index.js:38:8
	46 error     at ReadStream.<anonymous> (/usr/lib/node_modules/npm/node_modules/sha/index.js:85:7)
	46 error     at ReadStream.emit (events.js:117:20)
	46 error     at _stream_readable.js:943:16
	46 error     at process._tickCallback (node.js:419:13)
	````

You may be able to resolve it by executing `sudo npm cache clear`. Then, try re-installing.
