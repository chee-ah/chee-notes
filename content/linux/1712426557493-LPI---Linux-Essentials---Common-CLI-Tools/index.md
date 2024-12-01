---
title: "LPI - Linux Essentials - Common CLI Tools"
date: 2024-04-06
draft: false
description: "Notes while studying for LPI - Linux Essentials"
category: linux
series: ["LPI - Linux Essentials"]
series_order: 2
tags:
- LPI
- Linux Essentials
- Linux
- Study Notes
---

# CLI (Command Line Interface) Basics

## Basic concepts:

Everything in linux is always case-sensitive
```
.            means current directory
..           means parent directory
.something   at the beginning of a filename or folder means it is hidden
*            means everything
something.*  globbing (match anything that starts with 'something.')
```

## Permissions
```
drwxrwxrwx
||  |  |
||  |  \_ other permissions (r = read, w = write, x = execute)
||  \_ group permissions (r = read, w = write, x = execute)
|\_ user permissions (r = read, w = write, x = execute)
\_ file type (a dir in our case)
```

## Options vs Arguments
We've already explained this in the Basics, but here it is again, sometimes, it helps
re-wording things a little bit.

The input or stdin of a command line interface (cli) generally is given in the form of arguments.
Those arguments can be either positional or optional. Most of the time when you look at the help for
a program there is a usage description at the very top, this shows you what the program expects,
e.g. how many positional arguments there should be. generally positional arguments are mandatory,
and optional arguments are optional.

This is not always enforced, but a good practice is to have the options right after the program name
and the positional after.
```
- positional
- optional
```

The optional arguments start with a dash and one letter. One letter is the short form. But sometimes,
(not always) they also have a long form, denoted with a double dash --

Examples:
```
-h --help 
-v --verbose
-i --interface
-i --insensitive
-r --recursive
```

```
echo 'hello world'
   \  \_this is positionnal argument 1
    \_this is the program
```
```
nc localhost 443
 \  \         \_this is positionnal argument 2
  \  \_this is positionnal argument 1
   \_this is the program
```
```
nc -v localhost 443
 \  \  \         \_this is positionnal argument 2
  \  \  \_this is positionnal argument 1
   \  \_option v means verbose
    \_this is the program
```

## Basic commands
```bash
pwd     previous working directory (shows were you currently are)
rm      remove (careful it's dangerous)
rmdir   remove a directory (you could use rm for dirs too)
mkdir   make directory (creates a new directory)
touch   creates an empty file
```

## Cd

Change directory
```bash
cd      with nothing after also means go home
cd ~    go home
cd ..   go up in the directory structure: parent folder
cd -    go back in time: previous folder
```

## Ls

List files or directories
```bash
ls      list with no options
ls -a   list all (including hidden dot files)
ls -l   list in list format (more detailed)
ls -la  list all and in list format
ls -R   list recursively (all child folders contents)
ls -aR  list recursively and hidden files too
```

## Cp

Copy files or directories
```bash
cp          copy takes 2 arguments, source (src) and destination (dst)
cp src dst  would make a copy of 'src' into 'dst'
```

## Mv

Move files or directories
```bash
mv          copy takes 2 arguments, source (src) and destination (dst)
mv src dst  would transform (rename) 'src' into 'dst'
```

## Cat

Cat stands for concatenate, it just reads the content of a file straight to stdout
```bash
cat filename.txt
```

## Cut

Cut can be used to manipulate (cut) files
```bash
cat chee.txt 
hello my name is chee and i'm learning to make spaghetti
```
```bash
cat chee.txt | cut -d ' ' -f 1
hello
```
```bash
cat chee.txt | cut -d ' ' -f 2
my
```
```bash
cat chee.txt | cut -d ' ' -f 3
name
```
```bash
cat chee.txt | cut -d ' ' -f 3-
name is chee and i'm learning to make spaghetti
```
```bash
at chee.txt | cut -d ' ' -f -3
hello my name
```


Using `coma` as a delimiter
```bash
cat test.txt 
price,product,location
5.5,saussage,france
3.5,nemh,vietnam
```
```bash
cat test.txt | cut -d ',' -f 1
price
5.5
3.5
```
```bash
cat test.txt | cut -d ',' -f 2
product
saussage
nemh
```
```bash
cat test.txt | cut -d ',' -f 3
location
france
vietnam
```

## Head

We're creating a file with lines number 01 to 30 written in it
```bash
for i in {01..30};do echo line number $i >> data.txt;done
```
```bash
cat data.txt 
line number 01
line number 02
line number 03
line number 04
line number 05
line number 06
line number 07
line number 08
line number 09
line number 10
line number 11
line number 12
line number 13
line number 14
line number 15
line number 16
line number 17
line number 18
line number 19
line number 20
line number 21
line number 22
line number 23
line number 24
line number 25
line number 26
line number 27
line number 28
line number 29
line number 30
```

Head by default prints the first 10 lines of the file
```bash
head data.txt 
line number 01
line number 02
line number 03
line number 04
line number 05
line number 06
line number 07
line number 08
line number 09
line number 10
```

We can specify how many lines we want to print with the -n option
```bash
head -n15 data.txt 
line number 01
line number 02
line number 03
line number 04
line number 05
line number 06
line number 07
line number 08
line number 09
line number 10
line number 11
line number 12
line number 13
line number 14
line number 15
```

## Tail
Tail works the same except it prints the end or the file instead of the beginning
```bash
tail -n15 data.txt 
line number 16
line number 17
line number 18
line number 19
line number 20
line number 21
line number 22
line number 23
line number 24
line number 25
line number 26
line number 27
line number 28
line number 29
line number 30
```

## Grep

```
- grep stands for global regular expression and print
- so we're matching text strings in one or multiple files using regex (regular expressions)
- regex is a language which sole purpose is to match text
- and then we print that to stdout
```

```bash
grep chee file
grep chee ./*
grep -r chee file
grep -i chee file
grep -E '^[0-9]{2}:[0-9]{2}:[0-9]{2}' ./*
```
```
-i  - case insensitive (lower or upper case)
-r  - search recursively (in all directories)
-E  - extended regular expressions (full regex power)
```

## Tar

Tar is used to compress files and folders into archives (tar balls)

Synopsis
```
-c  create
-t  list contents
-x  extract
-v  verbose
-z  zip
-f  filename
```

create an archive called `archive-name.tar.gz` in `zip format` containing all `archive-file*.txt`
```bash
tar -cvzf archive-name.tar.gz archive-file*.txt
```

list the content of the archive without unpacking it
```bash
tar -tf archive-name.tar.gz
```
unpack (extract) the archive
```bash
tar -xvzf archive-name.tar.gz
```
