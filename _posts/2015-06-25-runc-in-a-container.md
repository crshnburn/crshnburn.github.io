---
layout: post
title: runC in a container
---

With the [recent announcement](http://blog.docker.com/2015/06/runc/) about the plumbing
for running containers being released to the [open containers group](https://github.com/opencontainers/runc)
I wanted to get stuck in and have a play and see what is involved.

Unfortunately (for me) it is still very much a Linux only proposition so not something I can build/run natively
on OSX, but I do have boot2docker so have containers ready to do the job. Compiling can be done using the instructions
available in the GitHub repository [https://github.com/crshnburn/docker-runc](https://github.com/crshnburn/docker-runc)

Once we have a container image with runC built and installed it's time to run a container to test it out.

The first thing that's needed is a file system for the container. Fortunately docker makes it easy to get one of these:
1. Run a container
```
docker run ubuntu
```
2. Get the ID of the container (which will have exited immediately)
```
docker ps -a
```
3. Export the file system
```
docker export <container_id> > rootfs.tar
```

Now we can build the image using the following Dockerfile,

```
FROM runc
MAINTAINER Andrew Smithson <smithson@uk.ibm.com>
RUN mkdir -p /container/rootfs
COPY . /container
WORKDIR /container
RUN tar -C rootfs -xf rootfs.tar
RUN /usr/local/bin/runc spec > container.json
ENTRYPOINT /usr/local/bin/runc
```

run a container using

```
docker run -it --rm --privileged -v /sys/fs/cgroup:/sys/fs/cgroup runc-test
```

and voila a runC container inside a Docker container. Not quite inception but certainly a portable way to start playing.
