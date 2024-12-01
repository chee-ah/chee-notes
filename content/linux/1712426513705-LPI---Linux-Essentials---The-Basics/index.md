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

## Variables

Our shell (bash) stores variables, variables are memory addresses where information is
stored for later use. We can use the `env` command to show all variables currently
defined in our shell.
```bash
env
```

We can also use `echo` against a variable to display its content specifically, for
instance here, the content of `$PATH`
```bash
echo $PATH
```

## Command locations and the $PATH Variable

In Linux everything is a file, even commands. So they are somewhere on the
filesystem just like any other file.

The system uses a variable called `$PATH` which is basically a place that remembers
all the filesystem paths where Linux will look for commands it can execute.
As stated before we can display the content of the variable with `echo`.
```bash
echo $PATH
```
```
/usr/local/bin:/usr/bin:/usr/local/games:/usr/games
```

This is simply a list of paths where Linux will look for commands binaries, each item in the list
is delimited by a semicolon, so the above $PATH will look in the following places:
```
/usr/local/bin
/usr/bin
/usr/local/games
/usr/games
```

If we go explore a little bit, in `/usr/bin` for instance we should see the actual binaries
for some commands we already learned about, and many, many more.
```bash
ls -la /usr/bin
```

Another very useful command to know about is the `which` command. It tells you which is the path of the
binary for any command, so in other words where is it in the filesystem, so if you check a few of the commands
you already know about you will se that in Debian, a lot of them are in `/usr/bin`
```bash
which ls
which uname
which which
```

## Command structure

In Linux cli commands are relatively homogenous convention. The command is always at the beginning of the line
and is sometimes followed by arguments. There are two types of arguments:
```
- positional arguments : they are not always required by can be, their position matters hence the name.
- optional arguments   : they are optional as the name implies, and their position does not matter,
                         but it is generally a good practice to put optional arguments immediately
                         after the command and before before the positional arguments if any.
                         Optional arguments can be in short form (prefixed with a single dash)
                         or in long form (prefixed with a two dashes)
```

Let's see some examples:

Echo does not technically require any arguments, if you give it none, it will just return a newline:
```bash
chee@vbox: $ echo

chee@vbox: $
```

But the point of echo is generally to write something, so most of the time we want to tell it what to write.
To do that we can give it a positional argument, in position 1, and it will write that to the terminal for us
```bash
chee@vbox: $ echo "hello world"
hello world
chee@vbox: $
```

It can a take both a positional argument and an optional argument, for instance `-n`
```bash
chee@vbox: $ echo -n "hello world"
hello worldchee@vbox: $
```
Something weird happened here, our prompt is not at the beginning of the line. Why?
Because the option -n means `no newline` it instructs echo to not put a newline character after writing.
think of a typewriter, we type something, but did not return the carriage to the beginning of the line yet.

Echo does not have long form options that I know of but `uname` does, long form options are the same as short form
except more descriptive let's see `--kernel-release` for instance does the same things as its short form `-r`
```bash
chee@vbox: $ uname --kernel-release
6.1.0-28-arm64
chee@vbox: $
```

Does the exact same thing as `-r`:
```bash
chee@vbox: $ uname -r
6.1.0-28-arm64
chee@vbox: $
```

`--help` and `-h` is also a relatively universal example of that.
```bash
uname --help
uname -h
```

Because every command it different the options and arguments they take are always different, so there's generally
not much point trying to remember them all, there are really a lot. A much smarter approach would be to get very
familiar with the documentation mechanism and to check them often. There is always really good and comprehensive
documentation in Linux, it's just a matter of knowing where to look. In fact there is a saying for that in the
Linux world: RTFM (Read The F****g Manual!).

So do read the manual, trust me, it's worth it, either use the `--help` or `-h` options for a quick nudge in the right
direction. Or if you need more comprehensive help you can use the `man` command, this stands for `manual` and can be used
as a prefix to any other command in linux, example: (q to quit)
```bash
man uname
```

A really useful trick with the man page, is to know how to search, it's quite simple, once you are in the manual page
you can use the character `/` to say search, and then the search term you are interested in.
So for instance in the ls man pages try to search for human-readable like this: `/human-readable` then press enter
and see if you can find what the correct options is to do that.

## Basic Commands
```
- echo 'hi' - write something on screen
- touch     - create file
- pwd       - previous working directory
- cd        - change directory (move home if no option)
- cd ..     - move up a directoryB
- cd -      - move back to the previous directory
- mkdir     - make directory
- ls        - list
- ls -l     - list in list format
- ls -la    - list all the files including hidden (dot files)
- ls -lt    - list sorted by time
- ls -ltr   - list sorted by time and reversed
- which     - find where a command's binary is on the filesystem
- date      - show the date
- uname     - show system version info
- uname -a  - show all system version info
- uname -r  - show system release version info
- env       - display all the environment variables in the current shell
- which ls  - see what is the the actual path for a given command, ls for instance here
```

## Brew to Install New Things on a Mac

```bash
- brew update           - update the `library` of available packages
- brew install vim      - install something new (vim here)
- brew upgrade          - upgrade all the things you already have
```

## Apt to Install New Things on a Debian system

```bash
- sudo apt-get update        - update the `library` of available packages
- sudo apt-get install vim   - install something new (vim here)
- sudo apt-get upgrade       - upgrade all the things you already have
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

## Linux Families and branches

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

## Up dog (a simple python server)
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
