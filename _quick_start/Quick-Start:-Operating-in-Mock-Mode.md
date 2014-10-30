---
layout: default
title: About
permalink: /about/
quick_start: true
---

# Operating in mock mode

Before we move along, we thought we'd call your attention to the Mock Mode feature of Apigee-127.

The handy and convenient Apigee-127 mock mode lets you "mock up" API routes/paths and response objects in the Swagger editor and test them without writing any controller code. By default, mock mode responses are system-generated; however, you can optionally implement custom mock controllers to return custom responses.

Just start your project with the `-m` Mock mode flag:

`a127 project start -m`

For details, see [Running Apigee-127 in mock mode](https://github.com/apigee-127/a127-documentation/wiki/Mock-mode).