# Serving DMN models

The core function of DSNTK is serving DMN models.
The DMN™ specification precisely defines XML interchange format of decision models.
XML files containing decision models are loaded and processed by DSNTK server and exposed
as a set of JSON endpoints.
Each endpoint represents a single invocable defined in the decision model.
Calling an endpoint is equivalent to executing a decision, business knowledge model
or decision service.

To explain in details, how to run and use the DSNTK server (a.k.a. _DMN runtime_),
we assume that DSNTK examples are saved in **examples** directory and the current
directory contains ONLY **examples** directory:

```shell
$ ls
examples
```

## Running DSNTK server

To run DSNTK server, enter the following command:

```shell
$ dsntk srv

Found 1 model.
Loaded 1 model.
Deployed 1 invocable.
dsntk 0.0.0.0:22022
```
DSNTK server started, accepts connections from all available network interfaces **0.0.0.0** (default)
and listens on port **22022** (default). During startup, the DSNTK server scans current working directory
(default) and searches for decision models stored in XML files having **dmn** extension.

During scanning, in the current working directory, the DSNTK server has found **examples** directory containing
one subdirectory with decision model file named **e2.dmn**. This file was loaded
and one invocable was deployed — a decision named **Greeting Message**.

This invocable can be evaluated by calling the endpoint 
```
examples/e2/io/dsntk/2_0001/Greeting%20Message
```

The list of all deployed invocables with endpoint names can be displayed during DSNTK server startup
by specifying the option **-v** or **--verbose**:

```shell
$ dsntk srv -v

Found 1 model.
Loaded 1 model.
Deployed 1 invocable.

Deployed invocables:
  examples/e2/io/dsntk/2_0001/Greeting%20Message

dsntk 0.0.0.0:22022
```

## Evaluating the invocable

Having already DSNTK server started, the deployed invocable can be evaluated by calling an endpoint
with required input data, e.g. using **curl**:

```shell
$ curl -s -d "{\"Full Name\":\"John Doe\"}" \
       -H "Content-Type: application/json" \
       -X POST http://0.0.0.0:22022/evaluate/examples/e2/io/dsntk/2_0001/Greeting%20Message
{"data":"Hello John Doe"}
```

DSNTK version of a [hello world](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program)
example could look like this:

```shell
$ curl -s -d "{\"Full Name\":\"World\"}" \
       -H "Content-Type: application/json" \
       -X POST http://127.0.0.1:22022/evaluate/examples/e2/io/dsntk/2_0001/Greeting%20Message
{"data":"Hello World"}
```

## Endpoint names

The common endpoint name for evaluating invocables exposed by DSNTK server is **evaluate**.

The full endpoint name (the URL) is built from the following parts:
- protocol, e.g.: **http://** or **https://**
- host address, e.g.: **0.0.0.0**, **127.0.0.1** or **my.domain.com**,
- common endpoint name: **evaluate**,
- directory names where the model file was found during scanning, e.g.: **examples/e2**,
- model namespace converted to RDNN-like path, e.g.: **io/dsntk/2_0001**,
- invocable name, e.g.: **Greeting Message**.

All parts of put together (and encoded to form a proper URL), give the following endpoint name:

```shell
http://127.0.0.1:22022/evaluate/examples/e2/io/dsntk/2_0001/Greeting%20Message
``` 


