---
title: "LPI - Install a Boot Manager"
date: 2024-12-15
draft: true
description: "Notes about Installing a boot manager"
series: ["LPI - Linux Essentials"]
series_order: 6
tags:
  - Linux
  - LPI
  - Linux Essentials
  - Study Notes
  - GRUB
  - Boot manager
---

# Install a Boot Manager - GRUB Legacy
To install Grub Legacy on a disc from a running system, we use the grub-install utility. Use the basic command grub-install device, where device is the disc. 
```bash
sudo grub-install /dev/sda
```
In this case sda is not the first partition sda1. By default, grub is going to copy the needed files to the /boot directory on that device.
If you want to copy it to the different directory, you can specify that using th boot-directory=parameter.
```bash
sudo grub-install /dev/sda --boot-directory=/other/directory
```
If you can't boost the system for some reason, and need to reinstall Grub Legacy, you can do that from the Grub Shell on the Grub Legacy boot disk. From the Grub shell, you would type the C-key at the boot menu to get the grub prompt. 
Step one : to set the boot device which contains the /boot directory of the first disk command be root (hd0,0). (That's because grub counts disks and partition starting with zero -> 0,0 first disk first partition)
if you don't know which device contains the boot directory, you can ask grub to search for it:
```bash
sudo grub> find /boot/grub/stage1

```
=> it'll show h(0,0) the first partition. One you find out what that is you would use the root command, and the setup command to install Grub Legacy to the master boot record and copy the needed files to the disk, 

```bash
sudo grub> setup (hd0)

```

Grub Legacy menu entries and settings are stored in a file at /boot/grup/menu.lst

# Notes from Butter bubble

Disk types
- SCSI, SATA, SAS are just drive technologies, what they mean is quite irrelevant right now
- sda, sdb, sdc ... means it's drive of type SCSI, SATA or SAS
- vda, vdb, vdc ... means it's a virtual drive typically on a virtual machine
- hda, hdb, hdc ... IDE Hard Drive (really old school tech)
- nvme0n1, nvme0n2 ... NVME new fast flash storage
- lots of other types

General explanations
- system V vs systems - systemd is the most common, you will see it everywhere on most modern systems
- mounting a disk is like creating a link, a bridge, for linux to know where the drive is, and how to interact with it
- chroot /mnt/sysroot is when you want to ask linux to pretend that the root of its system is somewhere else than it actually is
- recap the install disk automatically mounted the real OS disk in /mnt/sysroot and we are telling the install disk to pretend that’s the actual root, with chroot

The process of fixing grub as showed in the video
- This scenario implies that the normal linux system on disk is broken because of a grub config issue
- The BIOS (or UEFI) chooses where to look for a linux to start, we tell it to start on a CD Rom with a rescue disk
- When the disk is loaded by the BIOS the first thing it sees is GRUB Grand Unified Bootloader 
- The GRUB knows where to look for linux (still on the CD Rom in our case)
- Linux starts and systemd builds a working system out of the info on the CD Rom
- Now that linux is loaded the rescue disk automatically mounts the real disk (where the broken system is) on `/mnt/sysroot`
- we use the tool `chroot` (change root) to pretend that the real system is at `/mnt/sysroot`
- we use the tool `grub2-mkconfig` (grub2 make config) to rebuild the broken grub config
- once it's done we double check the disk layout with `lsblk` (list block) blocks meaning disks, or cd roms or usb storage, basically any storage device.
- boot loader files, like grub files are typically on a filepath called `boot` it's always the same, this way the `BIOS` knows where to look
- using `lsblk` we know the boot volume is on the `/sda` disk, now using `grub2-install` we install the grub software itself. 
- Now we can quit the install disk and it will reboot the machine, into the actual drive that we just fixed

Use `chroot`
```bash
chroot /mnt/sysroot
```

Use `grub2-mkconfig`, -o means output, we output a new config at `/boot/grub2/grub.cf` so essentially we re-generate it.

On a `BIOS` based system
```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
```

On a `UEFI` based system
```bash
grub2-mkconfig -o /boot/efi/EFI/centos/grub.cf
```

`lsblk` to list the block disk, and find that boot is on `sda`
```bash
lsblk
```

Install the grub software on `sda` for a BIOS system
```bash
grub2-install /dev/sda  
```

Install the grub software on a EFI system
```bash
dnf reinstall -y grub2-efi grub2-efi-modules shim 
```

Now we can quit the install disk and it will reboot the machine, into the actual drive that we just fixed
```bash
exit
```
