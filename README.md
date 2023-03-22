# Overview

This gets the Mosquitto broker along with the Topic Jail plugin up and running using [https://taskfile.dev/](https://taskfile.dev/).

This script and the created docker image is based upon:
* [cicorias/mosquitto-plugin-topic-jail](https://hub.docker.com/r/cicorias/mosquitto-plugin-topic-jail)

>Source code and GitHub actions that build, release, and publish an image are here:

* [cicorias/mosquitto-topic-jail-build](https://github.com/cicorias/mosquitto-topic-jail-build)

## Up and running

First ensure that [https://taskfile.dev/](https://taskfile.dev/) is installed.

Then from the root of the repository run the following:

```bash
task mosq
```

## Taskfile

Review the Taskfiles in `./Taskfile.yml` and `./certificates/Taskfile.yml` for all commands

### Installation of Taskfile

Run either the following or go to the homepage and review other options

```bash
sh -c "$(curl --location https://taskfile.dev/install.sh)" -- -d
```

```bash
sh -c "$(curl --location https://taskfile.dev/install.sh)" -- -d -b ~/.local/bin
```

### Need a tar file
This is missing cjson and other libs..

```dockerfile
# FROM cicorias/mosquitto-plugin-topic-jail:latest
FROM debian:bullseye-slim

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    ca-certificates \
    libcjson1 \
    net-tools inetutils-ping

ARG TARHOST=https://github.com/cicorias/mosquitto-topic-jail-build/releases/download
ARG TAGVERSION=v20230322_132600
ARG TARFILE=mosquitto_topic_jail.tar.gz

ADD ${TARHOST}/${TAGVERSION}/${TARFILE} /tmp/${TARFILE}

RUN tar -C /usr/bin -xzf /tmp/mosquitto_topic_jail.tar.gz && \
    mv /usr/bin/libmosquitto.so.1 /usr/lib/libmosquitto.so.1 && \
    rm /tmp/mosquitto_topic_jail.tar.gz

RUN mkdir -p /mosquitto/config
RUN useradd -ms /bin/bash mosquitto
# USER mosquitto
COPY mosquitto.conf /mosquitto/config/mosquitto.conf

CMD ["/usr/bin/mosquitto", "-c", "/mosquitto/config/mosquitto.conf", "-v"]

```