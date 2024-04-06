---
title: "Build a Webserver in Container Using Python and Updog"
date: 2024-04-06
draft: false
description: "Build a webserver in a debian container using python and updog"
category: docker
tags:
- docker
- server
- client
- updog
---

## Build a webserver in a container

Create a brand new debian container which exposes port 9090 on the local machine
```bash
docker run -it --name debian --rm --publish 9090:9090 debian /bin/bash
```

Update our package manager `apt`, meaning, refresh your list of known packages ready to be downloaded.
```bash
apt-get update
```

Use apt to install python in the container, the `-venv` means install python3, but with the module venv
```bash
apt-get install -y python3-venv
```

venv is a python module that we can use to build what's called a virtual environment, it's always a great practice when building python code to run in a virtual environment, I would recommend taking the habbit to always always do that. One venv = one app, and the point is to not mix the dependencies of 2 different apps in the same environment. You can think about it as taking a new blank piece of paper.
```bash
python3 -m venv venv
```

The command above created a virtual enviroment, this means have a new directory called venv, and in this folder we now have an entirely new copy of python.
```bash
cd venv
ls -l
cd ..
```

We now need to tell the system to use this isolated python for the work that we'll do from now on. This way we won't mix up our dependencies with the system python or any other apps that may be using it.
```bash
source venv/bin/activate
```

Now your prompt has changed and chows a (venv) prefix, this means we have loaded the venv, we're now in the virtual environment. We're in a blank page.  

So we can start building our app and installing the tools, we want to use updog, you'll see that a lot of things happen on screen, it tells you that it's collecting and installing a whole lot of things, those are the dependencies of updog, meaning the software libraries that updog needs to function. This is what pip does, it's a package manager, just like apt, but for python. So basically just a utility that knows if I need to run updog, I will need all those things for it to work.
```bash
pip install updog
```

So we can just double check that this works, and show the help of updog, if the help shows, we did everything right
```bash
updog --help
```

So now all that's left to do is start the webserver
```bash
updog
```

Or with tls/ssl, meaning with encryption over http, so https
```bash
updog --ssl
```

##  Expose the container port on LAN

Open another shell in your mac directly, and use netstat in combination with grep to list the listening ports on your machine. The container we just created is exposing it's port 9090 to the local machine, meaning your mac, and your mac is exposing 9090, on the network.  
```bash
netstat -tan | grep LISTEN
```

What does that mean? Think of doors, there are a bunch of doors on each computers, by default in linux we're not listening on any doors, this is why linux is so good at being secure. If you listen on any door, it's because you, the admin, has specifically configured a service, or system, or application, to listen behind that door.  

So essentially listening means we have a server, ready to serve a client. 
  - The server is providing a service
  - The client is consuming the service

Let's try it out, from now on we use 2 different computes on the same Local Area Network (LAN), they will have the prefix promps, client and server.  

On the server, on the mac not in the container, we need to find the ip address of the server. Because it's a mac we need to use ifconfig instead of ip a
```bash
server# ifconfig en0
```

We get back the ip address of the server, which is of course a private ip address, meaning it can only be used in the LAN. In our case right now it's `192.168.0.216`. So to sumarize, the server has the address 192.168.0.216 and it is listening on the port 9090, so, we know everything we need to actually go talk to that guy. 


## Use an other computer on the LAN as a client

On another computer, we can start a client to do that, our client app is called curl, it's stands for C url because it's written in C and it's a C library too. But essentially that's just a program that acts as an HTTP client.
```bash
client# curl http://192.168.0.216:9090
```

we'll see data comming from the updog server on the client side, and the server will see logs, telling it that someone is trying to get information from it. We have not offered any files yet though. So let's do that, I'll host those very notes, and you will pull them on your machines, so now I'm the server you're the client.  

I'm doing to the folder where my notes are and I'll start the server from here, effectively hosting the files that are in that folder
```bash
server# cd ~/www
```

currently my address is `192.168.0.31`, so you can reach me list this from the client side.
```bash
client# curl http://192.168.0.31:9090/notes.md
```
