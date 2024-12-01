---
title: "LPI - Linux Essentials - Shell Redirects"
date: 2024-04-06
draft: false
description: "Notes while studying for LPI - Linux Essentials"
category: linux
series: ["LPI - Linux Essentials"]
series_order: 3
tags:
- LPI
- Linux Essentials
- Linux
- Study Notes
- Shell Redirects
- Redirects
---

# Shell Redirects

A program always has the following:
```
- sdtin   - what is given to the program by the user
- sdtout  - what the program responds to the user when everything is fine
- sdterr  - what the program responds to the user when something's wrong
```

And in a diagram they look like this
```
             ---------
            |         | ---> stdout
 stdin ---> | Program | 
            |         | ---> stderr
             ---------
```

Think of redirects as `write to`
```bash
>     - take stdout and send it to a file (overwrite)
>>    - take stdout and send it to a file (append)
2>    - take stderr and send it to a file (overwrite)
2>>   - take stderr and send it to a file (append)
2>&1  - take stderr and send it to stdout (channel 1) then send stdout to a file
```

## >

example
```bash
echo haha > file
```

- the program is echo
- the stdin is haha
- the stdout is haha
- we redirected stdout to the file named file
- the redirection operator is only here once
- so it's overwriting the file every time
```
             ---------
            |         | ---> stdout  ---> file (overwrite)
 stdin ---> | Program | 
            |         | ---> stderr
             ---------
```

## >>

example
```bash
echo haha >> file
```

- the program is echo
- the stdin is haha
- the stdout is haha
- we redirected stdout to the file named file
- the redirection operator is here twice
- so it's NOT overwriting the file, it's appending to it every time
```
             ---------
            |         | ---> stdout  ---> file (append)
 stdin ---> | Program | 
            |         | ---> stderr
             ---------
```

## 2>

example
```bash
nc -vz localhost 80 2> file
```

- the program is netcat (nc)
- the stdin is `-vz localhost 80`
- the stderr is `Connection refused` (no stdout)
- we redirected stderr to the file named file
- the redirection operator is only here once
- so it's overwriting the file every time
```
             ---------
            |         | ---> stdout
 stdin ---> | Program | 
            |         | ---> stderr  ---> file
             ---------
```

### 2>>

example
```bash
nc -vz localhost 80 2>> file
```

- the program is netcat (nc)
- the stdin is `-vz localhost 80`
- the stderr is `Connection refused` (no stdout)
- we redirected stderr to the file named file
- the redirection operator is here twice
- so it's NOT overwriting the file, it's appending to it every time
```
             ---------
            |         | ---> stdout
 stdin ---> | Program | 
            |         | ---> stderr  ---> file
             ---------
```

## 2>&1

example
```bash
sudo tcpdump -i wlan0 dst google.com > file 2>&1

```

- the program is tcpdum
- the stdin is `-i wlan0 dst google.com`
- this command gives some stderr but also some stdout
- they are both displayed into the terminal by default (think walky-talky, listening on both channels)
- we redirect stderr (channel 2) to the background of stdout (channel 1)
- so all output eventually goes to stdout
- we then send stdout to the file
```
             ---------
            |         |             ---> stdout ---> file
 stdin ---> | Program |              \ 
            |         | ---> stderr --- 
             ---------
```
