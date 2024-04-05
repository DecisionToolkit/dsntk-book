# Getting started

^dt provides a set of examples ready to play with.

To install examples in **examples** directory, enter:

```shell
$ dsntk exs examples
```

^subcommand-exs-ref

## Evaluate example FEEL expression

```shell
$ cd ./examples/e1
$ dsntk efe e1.ctx e1.feel
```
```text
3
```

^subcommand-efe-ref

## Evaluate example DMN model

```shell
$ cd ./examples/e2
$ dsntk edm e2.ctx e2.dmn -i "Greeting Message"
```
```text
"Hello John Doe"
```

^subcommand-edm-ref

## Evaluate example decision table

```shell
$ cd ./examples/e3
$ dsntk edt e3.ctx e3.dtb
```
```text
0.15
```

^subcommand-edt-ref

## Run ^dt as a service

```shell
$ cd ./examples/e2
$ dsntk srv -v -H 127.0.0.1 -D .
```
```text
Found 1 model.
Loaded 1 model.
Deployed 1 invocable.

Deployed invocables:
  io/dsntk/2_0001/compliance-level-2-test-0001/Greeting%20Message

dsntk 127.0.0.1:22022
```

Switch to another terminal and enter:

```shell
$ curl -s -d "{\"Full Name\":\"John Doe\"}" \
       -H "Content-Type: application/json" \
       -X POST http://127.0.0.1:22022/evaluate/io/dsntk/2_0001/compliance-level-2-test-0001/Greeting%20Message
```
```text
{"data":"Hello John Doe"}
```

^subcommand-srv-ref
