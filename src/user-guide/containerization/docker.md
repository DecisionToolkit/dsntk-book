# Docker

[Dockerfile](https://github.com/dsntk/dsntk-rs/blob/main/Dockerfile)

```text
FROM scratch

# copy the binary file of Decision Toolkit to container root directory
COPY ./target/x86_64-unknown-linux-musl/release/dsntk /

# copy example DMN model to container root directory
COPY ./dsntk/src/examples/e2/e2.dmn /

# start DSNTK as a service and display all deployed invocables
CMD ["/dsntk", "srv", "--verbose"]
```

[docker.sh](https://github.com/dsntk/dsntk-rs/blob/main/docker.sh)

```shell
#!/usr/bin/env bash

# container name
NAME=dsntk

# container version
VERSION=0.0.3

# stop existing Docker container
docker stop $NAME

# remove stopped Docker container
docker rm $NAME

# remove existing Docker image
docker rmi "$(docker images | grep "^$NAME " | awk '{print $3}')"

# build the Decision Toolkit
cargo +stable build --release --target x86_64-unknown-linux-musl

# build new Docker image
docker build -t $NAME:$VERSION .

# start new Docker container
docker run --name $NAME -d -p 22022:22022 $NAME:$VERSION

# display logs from running Docker container
docker logs -f $NAME

# press Ctrl+C to stop following the log file ;-)
```

command

```shell
$ ./docker.sh
```
```text
  .
  . (log from executed commands)
  .
Found 1 model.
Loaded 1 model.
Deployed 1 invocable.

Deployed invocables:
  io/dsntk/2_0001/compliance-level-2-test-0001/Greeting%20Message

dsntk 0.0.0.0:22022
```

a

```shell
$ docker ps
```
result

```text
CONTAINER ID   IMAGE        COMMAND                  CREATED         STATUS         PORTS                     NAMES
fcb45779ce7d   dsntk:0.0.3  "/dsntk srv --verbose"   2 minutes ago   Up 2 minutes   0.0.0.0:22022->22022/tcp  dsntk
```
