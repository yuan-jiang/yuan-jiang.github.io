---
layout: post
title: Docker commands and files
date: 2017-07-04 14:37:00 +0800
tags: docker
---

Frequently used docker commands and examples of Dockerfile.

## Commands
{% highlight bash %}
# build image in current directory and tag it
$ docker build -t <image-name> .

# run an image in container
$ docker run -p <host-port>:<container-port> <image-name>
## useful options:
  # run in background as daemon
  1) -d
  # mount container path to outside host path
  2) -v /path/in/host:/path/in/container 

# start/restart container
$ docker start/restart <container-id>

# stop/kill container
$ docker stop/kill <container-id>

# list all containers
$ docker ps -a

# remove container
$ docker rm <container-id>

# list images
$ docker images

# remove images
$ docker rmi <image-id>

# execute command in container from host
$ docker exec <container-id> <cmd> [<cmd-args>]
{% endhighlight %}

## Dockerfile
{% highlight Dockerfile %}
# pull image
FROM image:tag
# e.g. FROM python:3.6.1-onbuild

# set working directory
WORKDIR /path/to/dir
# e.g. WORKDIR /var/app

# run commands if necessary (e.g. install dependencies)
RUN cmd
RUN ["executable", "param1", "param2"]
# e.g. RUN pip install -r requirements.txt

# add new files, directories or remote files into image
ADD src dest
ADD ["src", ..., "dest"]
# e.g. ADD . /var/app

# copy new files or directories into image
COPY src dest
COPY ["src", ..., "dest"]
# e.g. COPY app.py /var/app

# expose container port(s)
EXPOSE port [<port>]
# e.g. EXPOSE 5000 8000

# create a data volume in container at runtime
# a little hard to understand, reference below link:
# https://stackoverflow.com/a/27753725
VOLUME ["/path1", "/path2"]

# default app execution - only one per docker file
CMD cmd
CMD ["executable", "param1", "param2"]
# e.g. CMD gunicorn app:app -w 4
{% endhighlight %}

## References
- [docker commands](https://docs.docker.com/engine/reference/commandline/docker/)
- [docker images](https://hub.docker.com/explore/)
- [Dockerfile](https://docs.docker.com/engine/reference/builder/)
