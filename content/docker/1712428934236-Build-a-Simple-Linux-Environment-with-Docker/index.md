---
title: "Build a Simple Linux Environment with Docker"
date: 2024-04-06
draft: false
description: "Build a Simple Linux Environment with Docker"
tags:
- docker
- test environment
---
## Make a Linux test environment in Docker 

Apple's macOS is a Unix-like system, based on BSD, and as such is very similar to Linux. But technically, it's not linux, and it doesn't always have all the exact same tools we need to learn. When we want to experiment with those missing tools, we can use docker.  

Here's a really quick and simple guide on how to spin up a linux test environment in docker.  

Create a brand new docker container with debian
```bash
docker run -it --name debian debian /bin/bash
```

If we exit with ctr + d the container will be stopped but not deleted, we can list stopped containers like this:
```bash
docker ps -a
```

We can delete stopped containers we don't want to keep like that
```bash
docker rm debian
```

Otherwise here's how to restart our already existing debian docker container
```bash
docker start debian
docker exec -it debian /bin/bash
```
