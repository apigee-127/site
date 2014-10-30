---
layout: default
title: About
permalink: /about/
quick_start: true
---

# Quick start: Download and Start Usergrid

Usergrid is an Open Source project which is a Backend as a Service [(BaaS)](http://en.wikipedia.org/wiki/Mobile_Backend_as_a_service).  Usergrid is well suited to serve the needs of Mobile applications.  For more information about Usergrid please refer here: (http://usergrid.incubator.apache.org/)

This topic explains how to get started using Usergrid through a127.  With three short commands you can be up and running with Usergrid in your local environment:


```bash

$ a127 usergrid download

$ a127 usergrid start

$ a127 usergrid portal
```

## Prerequisites

In order to use Usergrid on your local machine you *must* have Java on your path.  Java 1.6+ is required.

## Downloading Usergrid
Usergrid is not downloaded by default when you download/install a127.  In order to download Usergrid you use the following command:

```bash
$ a127 usergrid download
Downloading Usergrid and Portal as needed...
................................................................................downloaded
- /Users/ApigeeCorporation/.a127/usergrid/usergrid-launcher-0.0.29.jar
- /Users- /ApigeeCorporation/.a127/usergrid/usergrid-portal-2.0.16/usergrid-portal/index.html
```

This will download a JAR file for Usergrid and an archive for the Usergrid portal and places them in the `~/.a127/usergrid` directory.  This is where the subsequent Usergrid commands rely on the artifacts to remain.

## Starting Usergrid
Usergrid can be started and stopped using a127 very easily.  Here is an example of starting Usergrid using a127:

```bash
$ a127 usergrid start
starting...
.............................................................started. (825)
Note:
  default Usergrid superuser: superuser/superuser
  default test-organization admin: test/test
```

### Starting Usergrid for the first time
When you start Usergrid for the first time using a127 a default configuration is laid out and some files are created.  Please find information about these files below:

- `~/.a127/usergrid/tmp` - This directory is created and files related to Usergrid configuration and storate are kept here.  If you want to 'start over' with Usergrid, you can stop Usergrid and remove this directory.  When you start Usergrid again this directory will be recreated.
- `~/.a127/usergrid/tmp/cassandra.yaml` - This is the configuration file for Cassandra which is used by Usergrid.  If for some reason you need to change the ports due to conflicts on your machine, this is where you would do it.
> This is NOT a typical use case
- `~/.a127/usergrid/tmp/data` - This is where the data stored in Usergrid is kept.  Depending on the amount of data you place in Usergrid, this directory may either consume a large or small amount of space.
- `~/.a127/usergrid/usergrid.log` - This is the log file for Usergrid/
- `~/.a127/usergrid/usergrid.pid` - This file is managed by a127 and should contain the Process ID of the Java process for Usergrid.  This is how a127 knows whether or not Usergrid is running.  If for some reason a127 does not see Usergrid running or if it thinks that Usergrid is running but it is not, this file (exsting or missing) may be the culprit



## Usergrid Portal
Usergrid has a management UI that can be used to configure and manage Usergrid.  a127 provides a *local* version of the Usergrid portal for you to use.

You can open the *local* Usergrid portal with the following command:

```bash
$ a127 usergrid portal
Opening browser to: /Users/ApigeeCorporation/.a127/usergrid/usergrid-portal-2.0.16/usergrid-portal/index.html
```

This will open your default browser and you should see a page like this:
![alt text](https://raw.githubusercontent.com/apigee-127/a127-documentation/master/a127/images/usergrid_portal_login.png)


You should be able to log in with test/test or superuser/superuse, after which you will see a page like this:
![alt text](https://raw.githubusercontent.com/apigee-127/a127-documentation/master/a127/images/usergrid_portal.png)

> Note: This requires the 'default' browser on your machine to be set.  This is typically done on Mac and Windows platforms for you.  If, however you are using Ubuntu then you will need to set your default browser manually.