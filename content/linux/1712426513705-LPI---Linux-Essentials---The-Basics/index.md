---
title: "LPI - Linux Essentials - The Basics"
date: 2024-04-06
draft: false
description: "Notes while studying for LPI - Linux Essentials"
category: linux
series: ["LPI - Linux Essentials"]
series_order: 1
tags:
- Linux
- LPI
- Linux Essentials
- Study Notes
---


## Basic Commands
```
- echo 'hi' - write something on screen
- touch     - create file
- pwd       - previous working directory
- cd        - change directory (move home if no option)
- cd ..     - move up a directoryB
- cd -      - move back to the previous directory
- mkdir     - make directory
- ls        -  list
- ls -l     - list in list format
- ls -la    - list all the files including hidden (dot files)
- ls -lt    - list sorted by time
- ls -ltr   - list sorted by time and reversed
- which     - find where a command's binary is on the filesystem
- date      - show the date
- uname     - show system version info
- uname -a  - show all system version info
- uname -r  - show system release version info
```

## Variables

Our shell (bash) stores variables, variables are memory addresses where information is
stored for later use. We can use the `env` command to show all variables currently
defined in our shell.
```bash
env
```

## Command locations and the $PATH Variable

In Linux everything is a file, even commands. So they are somewhere on the
filesystem just like any other file.

The system uses a variable called `$PATH` which is basically a place that remembers
all the filesystem paths where Linux will look for commands it can execute.
We can display the content of the variable.

## Command structure



## Brew to Install New Things on a Mac
```
- brew install    - install something new
- brew upgrade    - upgrade all the things you already have
```

## Terminal Signals
```
- cltr + l = clear
- cltr + c = kill
- cltr + d = disconnect
```

## Text Editors
```
- vim     - a text editor (pretty annoying an hard)
- nano    - a simple text editor (ctrl + x to quitw)
```

## Linux Famillies and branches

```
- Debian
  - Ubuntu
  - Kali Linux
  - Deepin
  - Pop OS

- Red Hat (RHEL)
  - Centos
  - Oracle Linux
  - Suze

- Other
  - Arch Linux
  - Alpine
  - Manjaro
  - ...
  - ..
  - .
```

## GitHub

```bash
git status
git add -u
git commit -m "Information about whatever you just added"
git push
```


## Curl (an http client)
```
- curl    - c (like the language) url (like an internet link) 
- curl https://example.com
```

## Updog (a simple python server)
```
- updog   - like what's up dawg
```

Start the server
```bash
updog 
```

Query the server
```bash
curl http://localhost:9090/chee.md
```

## Python
Here's how to install a python virtual environment
```bash
python3 -m venv venv
source venv/bin/activate
pip install -U pip
```

Then install a package in there
```bash
pip install updog
```

## Tmux
```
- tmux            = terminal multiplexer
- leader key      = ctrl + b
- leader + %      = split vertically
- leader + "      = splict horizontally
- leader + right  = go right
- leader + left   = go left
- leader + up     = go up
- leader + down   = go down
```
