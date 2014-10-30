---
layout: default
title: npm
permalink: /npm/
---

# Overview
By default npm will place 'global' modules installed with the `-g` flag in `/usr/local/lib/node_modules` using the default prefix of `/usr/local`.  Global executables would be placed in `/usr/local/bin` using the same default prefix, thereby putting them on the default PATH in most cases.  In order to write to both of these directories root permissions are required.

Many Node.js developers choose to use a different prefix such that they do not need to use root permissions to install modules using the `-g` flag (rightfully so - you should always be wary about things that 'require root permissions'!).  Using root permissions is effectively a shortcut.  In order to use executables installed using a different prefix you need to add an element to your path.

### Here are the steps:

1. Set the 'prefix' for npm by using the following command (documented here: [npm-config](https://www.npmjs.org/doc/misc/npm-config.html).  This will create a file `~/.npmrc` that contains configuration information for npm.

	```bash
	$ npm set prefix ~/npm
	```

2. Edit your `.bash_profile` or the appropriate shell initialization script to add `~/npm` to your `PATH` by adding the following line (or placing the single line in the new file if it does not exist):

	```bash
	PATH=~/npm/bin:$PATH
	```

	This will enable you to easily use executable scripts installed using `-g` through npm - both for Apigee-127 and for other tools as well!

>Note: If you have a problem, see the [Troubleshooting](https://github.com/apigee-127/a127-documentation/wiki/Troubleshooting-common-problems) page.