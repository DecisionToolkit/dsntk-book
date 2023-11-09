# Getting started

^dsntk provides a set of examples ready to play with.

To install examples in **examples** directory, run:

```shell
$ dsntk exs examples
```

## Evaluate example FEEL expression

```shell
$ cd ./examples/e1
$ dsntk efe e1.ctx e1.feel
3
```

## Evaluate example DMN model

```shell
$ cd ./examples/e2
$ dsntk edm e2.ctx e2.dmn -i "Greeting Message"
"Hello John Doe"
```

## Evaluate example decision table

```shell
$ cd ./examples/e3
$ dsntk edt e3.ctx e3.dtb
0.15
```

## Run ^dsntk as a service

```shell
$ cd ./examples/e2
$ dsntk srv -v -H 127.0.0.1 -D .
Found 1 model.
Loaded 1 model.
Deployed 1 invocable.

Deployed invocables:
  io/dsntk/2_0001/Greeting%20Message

dsntk 127.0.0.1:22022
```

Switch to another terminal and evaluate invocable:

```shell
$ curl -s -d "{\"Full Name\":\"John Doe\"}" \
       -H "Content-Type: application/json" \
       -X POST http://127.0.0.1:22022/evaluate/io/dsntk/2_0001/Greeting%20Message

{"data":"Hello John Doe"}
```
