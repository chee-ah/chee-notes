---
title: "Others - Notes about the bash shell"
date: 2024-05-06
draft: false
description: "Notes about the bash shell"
tags: ["bash", "shell", "notes", "other"]
---
# Bash Shells

## Different types of Shells

There are different types of shells in linux, popular ones are:
 * Bourne Shell (sh)
 * C Shell (csh or tsh)
 * Korn Shell (ksh)
 * Z Shell (zsh)
 * Bourne again shell (Bash)

## To check the shell being used:

```bash
echo $SHELL
```

## To change the default shell:
```bash
chsh
```

## Bash Shell Features

set custom aliases for the actual commands: 
```bash
alias name=value
alias dt=date
```

check the history of the shell
```bash
history
```

## Bash environment variables

read a variable
```bash
$echo $SHELL
```

read all environment variables:
```bash
env
```
## Path Variable:

see the directories defined in path variable:
```bash
echo $PAT
```

check if the location of the command can be identified:
```bash
which bash
```
