---
layout: default
title: Configs Options
permalink: /config-opts/
---

## Configuration options

This topic explains miscellaneous configurations that you can make for an Apigee-127 app.

###Configuring the default browser on Linux

On Linux platforms, you need to specify your browser path before using the Swagger editor.

1. Create or open the following file in a text editor:

    `~/.a127/config.js`

2. Add the following contents to the file:

    ```javascript
        module.exports = {
           browser: ’the/path/to/your/browser'
        };
    ```
