---
layout: default
title: About
permalink: /about/
quick_start: true
---

# Deploying your Apigee-127 project to Apigee Edge

* [Why deploy to Apigee Edge?](#why)
* [Prerequesites](#prerequisites)
* [Basic steps](#basic)
* [Detailed description](#description)
* [Test your app on Edge](#test)
* [Examples](#examples)

## <a ref="why"></a>Why deploy to Apigee Edge?

When Apigee-127 applications are deployed to the Apigee Edge platform, they will take advantage of local optimizations so that they run with the best possible availability, scalability, and performance.

## <a ref="prerequisites"></a>Prerequisites

To deploy your Apigee-127 project to Apigee Edge you'll need:

* An account on [Apigee.com](https://accounts.apigee.com/accounts/sign_up)
* An Apigee-127 account configured with your Apigee.com account information.
   For example: `$ a127 account create <my_account>`

>Note that if you do not have an Apigee.com account, you'll be prompted to create one when your run this command.

## <a ref="basic"></a>Basic steps

If you do not have one, you must create an Apigee-127 account with your Apigee Edge account information. Enter this command and follow the prompts:

    a127 account create <my-account>

From the root directory of your Apigee-127 project, run:

    a127 project deploy

Apigee-127 bundles up your project and deploys it to Edge. It uses the Apigee account information stored in the currently active Apigee-127 account.


## <a ref="description"></a>Detailed description

The `$ a127 project deploy` command uses `apigeetool` behind the scenes to deploy your Apigee-127 project to Apigee Edge.

The `$ a127 project deploy` command includes these options:

* -h or --help
    >Prints usage information to standard output

* -a or --account [account]
    >Lets you specify which Apigee-127 account to use. By default, the command takes the currently active account. To see which account is active, do `a127 account list`.

* -i or --import-only
    >Imports the project to Edge, but does not deploy it.

* -n or --name [name]
    > Overrides the default name for the project. The default name is `a127-<project_root_folder_name>`. For example, `a127-example-project`.

* -main or --main [main]
    > Overrides the main Node.js application file. In a new Apigee-127 project, the main file is `<project-folder>/app.js`.

* -b or --base [path]
    >Overrides the base path for the Apigee Edge API proxy deployment. By default, the base path is `/<project-name>`.

* -d or --docs [path]
    >Overrides the base path from where your API's documentation is accessed. The default is `/docs`.

## <a ref="test"></a>Test your API on Edge

`$ curl http://ORG-ENV.apigee.net/PROJECT-NAME`

Substitute the name of your organization on Apigee Edge, the name of the environment to deploy to (e.g., test), and the name of the deployed project. By default, the name is `a127-<project-name>`, where `<project-name` is the name of your Apigee-127 project. You can specify a different name if you deploy with the -n option.

##<a ref="examples"></a>Examples

`$ a127 project deploy -n myproxy`

>Deploys the project to Apigee Edge as an API proxy named `myproxy`.

`$ a127 project deploy -m myfile.js`

>Deploys the project to Apigee Edge and configures the main Node.js file to be `myfile.js`.

