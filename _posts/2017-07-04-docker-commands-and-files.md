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
$ docker build -t my-image .

# run an image in container
$ docker run -p host-port:container-port my-image
## useful options:
  # run in background as daemon
  1) -d
  # mount container path to outside host path
  2) -v /path/in/host:/path/in/container 

# start/restart container
$ docker start/restart container-id

# stop/kill container
$ docker stop/kill container-id

# list all containers
$ docker ps -a

# remove container
$ docker rm container-id

# list images
$ docker images

# remove images
$ docker rmi image-id
{% endhighlight %}

## Dockerfile
{% highlight Dockerfile %}
# pull image
FROM image:tag

# set working directory
WORKDIR /path/to/dir

# run commands if necessary (e.g. install dependencies)
RUN cmd
RUN ["executable", "param1", "param2"]

# add new files, directories or remote files into image
ADD src dest
ADD ["src", ..., "dest"]

# copy new files or directories into image
COPY src dest
COPY ["src", ..., "dest"]

# expose container port(s)
EXPOSE port [<port>]

# mount container paths to host or other containers
VOLUME ["/path1", "/path2"]

# default app execution - only one per docker file
CMD cmd
CMD ["executable", "param1", "param2"]
{% endhighlight %}

## References
- [docker commands](https://docs.docker.com/engine/reference/commandline/docker/)
- [docker images](https://hub.docker.com/explore/)
- [Dockerfile](https://docs.docker.com/engine/reference/builder/)
