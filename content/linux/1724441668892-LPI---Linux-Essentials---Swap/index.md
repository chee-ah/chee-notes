---
title: "LPI - Linux Essentials - Swap"
date: 2024-08-23
draft: false
description: "Notes about swap files or partitions"
series: ["LPI - Linux Essentials"]
series_order: 5
tags:
- LPI
- Linux Essentials
- Linux
- Study Notes
- Swap
---

# Swap 

quick reminder:

| what   | where                       | conceptually | example          |
| ------ | --------------------------- | ------------ | ---------------- |
| memory | ram (random access memeory) | ephemeral    | program variable |
| disk   | file system                 | persistent   | file             |

Swap files are a way to use the file system for the same purpose as ram, when the ram is full.

## Design hard disk layout and manage swap space:
- Configure and Manage Swap Space: 

- Create a Swap space:
Swap is an area where Linux can temporarily move some data from the computer's random access memory or RAM. A swap partition is essentially an extension of the RAM, on disk.

To check if the system uses any kind of swap areas:
```bash
swapon --show
```

If one partition is used already as swap, we can add more partitions if we want:

First, have a look at what partitions we have available, using lsblk:
```bash
lsblk
```

Then, we can create a partition at... Specifically to be used as swap. 

Before it has to be used as a swap, it has to be prepared. Basically, it is a similar process with formatting a USB stick, write some small data on the partition, labels it, now the system knows this is meant to be used as swap area. 

To do it:
- Tell Linux to use this partition as swap:
```bash
sudo mkswap /dev/vdb3
```
```
*Setting up swap space version 1, size = 2 GiB (2146430976 bytes) no label, UUID=6d6f451e-5fa4-4cd5-b627-b0f39c810002*
```

- Use the swapon command verbose option makes the command output more detailed about what we are doing.
```bash
sudo swapon --verbose /dev/vdb3
```
```
*swapon: /dev/vdb3: found signature [pagesize=4096, signature=swap]*
*swapon: /dev/vdb3*
```

```bash
swapon --show
```

- But if we reboot this systems, the partition won't be used as swap anymore, we'll see later how to tell Linux to automatically use this as swap every time.

- To stop using our partition as a swap space:
```bash
sudo swapoff /dev/vdb3
```

- Instead of using partitions, we can also use a simple file as swap.
 First, we need to create an empty file and fill it with zeros. (binary zeros).

 We will use a utility called dd, and we will have it use these parameters. 
```bash
 sudo dd if=/dev/zero of=/swap bs=1M count=128
```
- `sudo`: SuperUserDO just do things as root
- `if=/dev/zero`: Input `/dev/zero` that is a device generating zeros
- `of=/swap`: To tell `dd` where to write the output. 
- `bs=1M`: Means block size and we chose a block size of 1M
- `count=128`: count means we are counting 128 times a block of 1M

So Basically, we create a file called swap and using a device with generates an infinite number of zeros, we fill up the file with zeros, counting 128 * 1 Megabyte until we stop, so of course our file will be 128MB exactly in the end, we can double check that with the following command (l=list, h=human):
```bash
ls -lh /swap
```
