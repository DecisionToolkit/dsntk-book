# Serving DMN models

The core function of ^dt is serving DMN models.
The ^dmn specification precisely defines XML interchange format of decision models.
XML files containing decision models are loaded and processed by ^dt server and exposed
as a set of [JSON API](https://jsonapi.org) endpoints.
Each endpoint represents a single invocable defined in the decision model.
Calling an endpoint is equivalent to executing a decision, business knowledge model
or decision service.

To explain in details, how to run and use the ^dt server (a.k.a. _DMN runtime_),
we assume that ^dt examples are saved in **examples** directory and the current
directory contains ONLY **examples** directory:

```shell
$ ls
examples
```

## Running ^dt server

To run ^dt server, enter the following command:

```shell
$ dsntk srv

Found 1 model.
Loaded 1 model.
Deployed 1 invocable.
dsntk 0.0.0.0:22022
```
^dt server is started, accepts connections from all available network interfaces **0.0.0.0** (default)
and listens on port **22022** (default). During startup, the ^dt server scans current working directory
(default) and its subdirectories, and searches for decision models stored in XML files having **.dmn** extension.

During the scanning, in the current working directory, the ^dt server has found **examples** directory
containing one subdirectory named **e2** with decision model file named **e2.dmn**. This file was loaded
and one invocable from this file was deployed — a decision named **Greeting Message**.

This invocable can be evaluated by calling the endpoint:

```text
examples/e2/io/dsntk/2_0001/compliance-level-2-test-0001/Greeting%20Message
```

The list of all deployed invocables with endpoint names can be displayed during ^dt server startup
by specifying the option **-v** or **--verbose**, like shown below:

```shell
$ dsntk srv -v
```
```text
Found 1 model.
Loaded 1 model.
Deployed 1 invocable.

Deployed invocables:
  examples/e2/io/dsntk/2_0001/compliance-level-2-test-0001/Greeting%20Message

dsntk 0.0.0.0:22022
```

## Evaluating the invocable

Having the ^dt server started, the deployed invocable can be evaluated by calling an endpoint
with required input data, using [**curl**](https://curl.se) for example:

```shell
$ curl -s -d "{\"Full Name\":\"John Doe\"}" \
       -H "Content-Type: application/json" \
       -X POST http://0.0.0.0:22022/evaluate/examples/e2/io/dsntk/2_0001/compliance-level-2-test-0001/Greeting%20Message
```
```text
{"data":"Hello John Doe"}
```

The ^dt version of a [hello world](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program)
program could look like this:

```shell
$ curl -s -d "{\"Full Name\":\"world\"}" \
       -H "Content-Type: application/json" \
       -X POST http://127.0.0.1:22022/evaluate/examples/e2/io/dsntk/2_0001/compliance-level-2-test-0001/Greeting%20Message
```
```text
{"data":"Hello world"}
```

## Endpoint names

The common endpoint for evaluating invocables exposed by ^dt server is named: 

> **evaluate**

The full URL of the endpoint is built from the following parts:
- protocol:
  
  **http://** or **https://**

- host address, like:

  **0.0.0.0** or **127.0.0.1** or **my.domain.com**

- common endpoint name:
 
  **evaluate**

- directory names where the file containing DMN model was found during scanning:

  **examples/e2**
 
- model namespace converted to RDNN-like path:
 
  **io/dsntk/2_0001/compliance-level-2-test-0001**

- invocable name:
  
  **Greeting Message**

All parts put together, give the following URL of the endpoint:

> **http://127.0.0.1:22022/evaluate/examples/e2/io/dsntk/2_0001/Greeting%20Message**
 
> ^note While not all characters are legal in URL, there is **%20** between **Greeting** and **Message**,
> which in [percent-encoding](https://en.wikipedia.org/wiki/Percent-encoding) represents a space.
> See [RFC3986](https://datatracker.ietf.org/doc/html/rfc3986#section-2.4) for more details.
