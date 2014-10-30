---
layout: default
title: Understanding Swagger
permalink: /understanding-swagger/
---

* [What is Swagger?](#whatisswagger)
* [How does Apigee-127 use Swagger?](#howdoes)
* [Help me with YAML](#helpwith)
* [Next step](#nextstep)


## <a name="whatisswagger"></a>What is Swagger?

[Swagger™ ](https://helloreverb.com/developers/swagger) is a specification and framework implementation for describing, producing, consuming, and visualizing RESTful web services.

To read more about Swagger, refer to:

* [The Swagger website](https://helloreverb.com/developers/swagger)
* [Swagger on GitHub](https://github.com/wordnik)


## <a name="howdoes"></a>How does Apigee-127 use Swagger?

The Swagger Editor for Apigee-127 lets you design your API specification and preview its documentation for your Apigee-127 API. This editor is part of Apigee-127, and it is installed with Apigee-127.

Use this editor to configure the `swagger.yaml` configuration file. A basic version of the file is provisioned with every new Apigee-127 project, and lives in `<project_root>/api/swagger/swagger.yaml`. It conforms to the [Swagger 2.0 specification](https://github.com/reverb/swagger-spec/blob/master/versions/2.0.md).

Behind the scenes, Apigee-127 Swagger middleware validates and processes the Swagger configuration file, and routes API operation endpoints to controller files. All **you** need to do is implement your custom API controller logic.

>Note: The editor is served locally and automatically saves your work *as you edit*. In addition, if the project is running (`a127 project start`), it is automatically restarted each time the editor saves. Just be careful if you kill the editor, that you do not lose unsaved work.

**Try it:**

1. `a127 project create test-project`
2. `cd test-project`
2. `a127 project edit`

*The Swagger editor for Apigee-127*
![alt text](https://raw.githubusercontent.com/apigee-127/a127-documentation/master/a127/images/swagger-editor.png)


## <a name="helpwith"></a><a name="yaml"></a>Help me with YAML

YAML is a data serialization/representation standard. If you're new to YAML, check out [www.yaml.org](http://www.yaml.org). Another excellent introduction is the [Wikipedia YAML entry](http://en.wikipedia.org/wiki/YAML).

YAML is intended to be easy for humans to read. Every Apigee-127 project includes a Swagger 2.0 compliant configuration file that is written in YAML.

## <a name="nextstep"></a>Next step

Now that you know a little about Swagger, go to the next topic "[The Swagger specification file](https://github.com/apigee-127/a127-documentation/wiki/Swagger-specification-file)" for details on the elements and extensions you need to know about to build an Apigee-127 model.