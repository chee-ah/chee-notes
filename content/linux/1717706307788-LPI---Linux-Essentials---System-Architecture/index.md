---
title: "LPI - Linux Essentials - System Architecture"
date: 2024-06-06
draft: false
category: linux
description: "Notes about system achitecure in relation to my Linux Essential studies"
series: ["LPI - Linux Essentials"]
series_order: 4
tags:
- LPI
- Linux Essentials
- Linux
- Study Notes
---

# Create a multipass:
```bash
brew install multipass
```

```bash
multipass list
```
```bash
multipass shell chee-vm
```

# Log out:
Control D
=> back:
```bash
multipass shell chee-vm
```


# Determine and Configure Hardware setting:

## BIOS = Basic Input/Output System

Set the date and time for the hardware clock
Disable or enable integrated peripherals
Configure error protection
Change hardware settings like IRQs and DMA
Choose the order of boot devices
* Inspecting Devices in Linux
Once Linux loads, the operating system needs to take the necessary steps to make sure hardware can be used. If the hardware isn't detected by the operating system or the port into which it's been plugged is defective  but if a piece of hardware is correctly detected by the operating system and isn't working properly we need to take additional steps like making sure the right kernel module or driver is loaded.
- To show all the device that are connected to the peripheral Component Interconnect bus (PCI bus)
```bash
sudo lspci
```

when we want to focus a bit better on some extra of the information that we can see about the device on the system by using the-s option with LSPCI followed by the address of the piece of hardware  -v or verbose flag
```bash
sudo lspci -s 00:03.0 -v
```

In addition to the hardware address the component, we can also see the subsystem field which shows the brand and model. We can also see the kernel driver that is in use (kernel module).
If we want to narrow the output down to focus on the kernel module being used, we can use the -k flag along with the hardware address

```bash
sudo lspci -s 00:03.0 -k
```

- To identify devices that are connected by usb
```bash
sudo lsusb
```

For more detail, we can add the -v flag just like with LSPCI. Or we can also specify a device ID by using the -d option
```bash
sudo lsusb -v -d 1781:0c9f
```

Devices can be viewed in a hierarchical or tree format by using the -t option
to get specific information about particular device, we can use the -s flag, followed by the bus and dev numbers for the device
```bash
sudo lsusb -s 01:20
```
(bluetooth adapter)

- To see the list of kernel modules currently loaded on the system:
```bash
sudo lsmod
```

To look at the module name, we can use a utility `fgrep` to search and narrow things down:
```bash
sudo lsmod | fgrep -i snd
```

If we are having trouble with a piece of hardware we might want to unload the kernel module that is currently in use:(-r: remove)
```bash
sudo modprobe -r snd-hda-intel
```

Kernel module also have parameters that can be changed  and each module has its own specific parameters. Most of the time we want to keep the default options. We can see module information by its author, license, identification,...
```bash
sudo modinfo snd
```

we can make persistent changes to these parameters by setting them in a file located at etc/modprode.conf or creating individual files with the .con extension on the directory /etc/modprode.d./snd.conf
If the module is causing a problem , we can block the kernel from loading it by specifying it in a special located in etc/modprobe.d/blacklist.conf    we would just add an entry in that file that reads like this blacklist snd, to blacklist the snd module to keep it from loading.
## UEFI = Unified Extensible Firmware Interface

## POST = Power-On Self-Test

# Boot the system:

## BIOS or UEFI  --> Boot-loader (bootstrap) ---> Kernel ---> Init:

When the BIOS loads, the following four steps take place:
1) POST Identifies simple hardware failures
2) Activates basic components like video output, the keyboard, and storage
3) Loads the bootstrap from the MBR
4) Loads the second stage of the boot-loader and pass options to the kernel

UEFI: Unified Extensible Firmware Interface differs from BIOS in several key ways:
- NVRAM
- EFI applications
- FAT file-systems or ISO-9660
- EFI System Partition (ESP)
- EFI directory

Doesn't reply on an MBR but instead looks for settings stored in its non-volatile memory, or NVRAM. These setting contain the location of UEFI-compatible programs called EFI applications which can be executed automatically or called from a boot menu. These can be boot-loader, operating system selectors, or diagnostic utilities. The EFI applications must be on a regular storage device partition using a compatible file system.

When UEFI loads:
1) POST Identifies simple hardware failures
2) Activates basic components like video output, the keyboard,
and storage
3) Executes EFI applications stored in the ESP partition, such as
the boot-loader
4) The boot-loader loads the kernel
5) Also supports Secure Boot which is only allows signed EFI applications to execute

GRUB (for the x86 architecture boot-loader): Grand Unified Boot-loader
SHIFT if the system is using BIOS  and ESC if the system is using UEFI to see that link menu

## Most useful kernel parameters:
```
- acpi:         Enable or disable ACPI support  Example: acpi=off disables
- init:         Set system init                 Example: init=/bin/bash
- systemd.unit: Set systemd target              Example: system.unit=graphical.target
- mem:          Set available system RAM        Example: mem=512M
- maxcpus:      Limits processors or cores      Example: maxcpus=0 or maxcpus=2
- quiet :       Hides boot messages
- vga:          Selects a video mode            Example: vga=ask
- root:         Sets the root partition         Example: root=/dev/sda3
- rootflags:    Mount options for the root filesystem
- ro Mount:     root filesystem read-only
- rw Mount :    root filesystem read/write
```

## Kernel loaded into RAM:
Kernel mounts all file-systems configured in /etc/fstab
Kernel loads init. First program that starts all others.
initramfs is removed from RAM

## Different Init types:
- SysV standard Controls daemons using runlevels. Runlevels numbered 0 to 6.
- systemd Modern service manager with concurrent structure. Common default.
- Upstart Parallel startup. Formerly used by Ubuntu, replaced by systemd.

## To see the message in the kernel ring buffer:
```bash
dmesg
```

On systems that use systemd, the journalctl command can be used to show the initialization messages:
```bash
journalctl --list-boots
```

Using the -b or the --boot=options to specify a boot number and see messages from that initialization.
- For example: `journalctl -b 0`, `-b 0` is going to show us information for the current boot.

The operating system is going to store initialization messages and other log entries in the directory `/var/log`.

`/var/log/` initialization and system logs.
```bash
journalctl -D /var/log/other_directory
```

# Change Runlevels/ Boot Target and Shutdown or Reboot System:

## SysVinit

PID 1 The service manager process. Runlevel A group of services for a purpose. Numbered 0 to 6.
- Runlevel 0 System shutdown.
- Runlevel 1 Single-user mode, without networking. Maintenance mode.
- Runlevel 2,3,4 Mulit-user mode. Networking available. 2 and 4 often unused.
- Runlevel 5 Multi-user mode with graphical login.
- Runlevel 6 System restart.

/sbin/init Manages runlevels and services.

Set Multi-user.target default when start:
```bash
Systemctl set-default multi-user.target
```

- Graphical.target:
```bash
systemctl set-default graphical.target
```

- Check the current boot default mode:
```bash
systemctl get default
```

- Temporarily switch from graphical -> multi-user:
```bash
systemctl isolate multi-user.target
```

- multi-user -> graphical:
```bash
systemctl isolate graphical.target
```
or
```bash
init 5
```

- /etc/inittab:

`/etc/inittab` defines each runlevel. `/etc/init.d/` contains scripts for each runlevel.

`id:runlevels:action:process`

Id is a generic name up to four characters in length used to identify the entry. The runlevels entry is a list of runlevel numbers to perform a specific action. The action term determines how init will execute the process specified by the process term. The available actions are:
- boot Executed during system initialization. Ignores runlevels field.
- bootwait Executed during system initialization and init waits until finished.
- Ignores runlevels field.
   sysinit Executed after system initialization. Ignores runlevels field.
- wait Executed for the given runlevels. Init waits until finished.
- respawn Process will be restarted if terminated.
ctrlaltdel Executed when init receives SIGINT, triggered by CTRL+ALT+DEL.
- `id:x:initdefault` Defines default run-level.

If we open etc/inittab with editor like vi,
```bash
sudo vi /etc/inittab
```

If we make a change to the `/etc/inittab` file after we save and exit our editor, we run `telinit` with the  q option to have init reload its configuration so it's aware of those changes
```bash
sudo telinit q
```

The scripts used by each level are stored in /ect/init.d directory . If we take a look inside of that directory, we will see subdirectories of each runlevel labeled.
```bash
ls /etc/init.d/
```

The same script may be present in more than one subdirectory, if it runs in different levels, the first letters of scripts file name will indicates whether the script is mean to stop a service, or start the service when it runs.
*Files that start with K kill services when they run*
*Files that start with S start services when they run*
If we use the runlevel command , without any parameters, it will tell us our current system runlevel
```bash
runlevel
```

The first value it shows is the previous system runlevel, for example: N 3,   a letter N mean the runlevel has not changed since the last system boot, and the second number is the current runlevel , in ths case, 3.
We can switch between runlevel using the telinit command , for example:
```bash
sudo telinit 1
```

## Systemd:
Systemd is now the most used set of tool to manage resources and service in Linux. Systemd refers to resources and services as units. A systemd unit will have a name, a type, and a configuration file. For example, an Apache Web server would have an>a unit of http.service on Red Hat systems.

httpd.service (Red Hat) or apache2.service (Debian)
```
service:    Active system resources. Can be initiated,
            interrupted, and reloaded.
socket:     Filesystem or network socket.
device:     A hardware device identified by the kernel.
mount:      A mount point defined in /etc/fstab.
automount:  A mount point mounted automatically.
target:     A group of units managed as a single unit.
snapshot:   A saved stated of the systemd manager.
```
The main command for controlling systemd units is systemctl. The command systemctl is use to execute all tasks regarding unit activation: deactivation,execution,interruption,monitoring, ....
For the fictitious unit called unit.service, for example:

- Start the unit:
```bash
sudo systemctl start unit.service
```

- Stop the unit:
```bash
sudo systemctl stop unit.service
```

- Restart a unit:
```bash
 sudo systemctl restart unit.service
```

- Show the status of a unit, including if it is running or not:
```bash
 sudo systemctl status unit.service
```

- Show if the unit is running or inactive if it is not:
```bash
 sudo systemctl is-active unit.service
```

- Enable a unit: means that it is going to load whenever the system boots up
```bash
 sudo systemctl enable unit.service
```

- Disable a unit: make sure a unit will not start with the system
```bash
 sudo systemctl disable unit.service
```

- The is-enabled verifies if a unit is going to start with the system. The answer is stored in a variable. The value of zero indicates that the unit is going to start with the system. The value 1 indicates that the unit does not start with the system.
```bash
 sudo systemctl is-enabled unit.service
```

The command systemctl can also control the system targets.
The multi-user.target, for example combines all units required by the multi-user system environment, that's like running level 3 in the system using system 5

```bash
sudo systemctl isolate multi-user.target
```

 .Set the default target:

```bash
sudo systemctl set-default multi-user.target
```
. Get/determine the default
 ```bash
$ sudo systemctl get-default
```
(*graphical.target*)
We can also use `systemctl isolate` to manually change just like you would use the telinit command in system 5. Like systemd copying SystemV, the default target should never point to shutdown.target, as it is going to correspond to run level zero. The configuration files associated with every unit can be found in this directory: `/lib/systemd/system/` contains unit files for every unit.
```bash
sudo systemctl list-unit-files
```

We can specify the type of option to ony select units for a given types:
```bash
sudo systemctl list-unit-files --type=service
```

We could specify services or target to be shown:
 ```bash
sudo systemctl list-unit-files --type=target
```

Active units or units that have been active during the current system session can be list with the ctl init units. It also responsible systemd for triggering and responding  to power related events . This suspend command below will put the system in low-power mode, keeping data in memory
 ```bash
sudo systemctl suspend
```

To hibernate, copy all memory data to disk so the current state of the system can be recovered after it's powered back on.

 ```bash
sudo systemctl hibernate
```

This actions associated with these type of events are defined in the file `/etc/systemd/logind.conf` defines action associated with power events. And it can also be found in separate files in `/etc/systemd/logind.conf/d/` if no other power , manager like acpid is running on the system

## Upstart:
The initialization scripts used by Upstart, they are going to be in the directory `/etc/init/` contains initialization scripts for Upstart.
Upstart is a discontinued event-based replacement for the traditional init daemon, the method by which several Unix-like computer operating systems perform tasks when the computer is started.

- List the system services:
```bash
sudo initctl list
```

It is going to show the current state of the services. And if it's available, their PID number.
```
avahi-cups-reload stop/waiting
avahi-daemon start/running, process 1123
mountall-net stop/waiting
mountnfs-bootclean.sh start/running
nmbd start/running, process 3085
passwd stop/waiting
rc stop/waiting
rsyslog start/running, process 1095
tty4 start/running, process 1761
udev start/running, process 1073
upstart-udev-bridge start/running, process 1066
console-setup stop/waiting
irqbalance start/running, process 1842
plymouth-log stop/waiting
smbd start/running, process 1457
tty5 start/running, process 1764
failsafe stop/waiting
```

Every Upstart action has its own independent command, for example: The start command can be used to initialize the sixth virtual terminal:
```bash
start tty6
```

The current status of a resource can be viewed, verified with the status command:
```bash
status tty6
```

tty6 start/running, process 3282
And interrupting a service can be done with the stop command:
```bash
stop tty6
```

Upstart does not use the `/etc/inittab` file to determine runlevels, but the legacy commands runlevel and telinit can still be used to verify and alternate (luan phien) between runlevels.

## Shutdown and Restart:

```bash
sudo shutdown [option] time [message]
```

```bash
sudo shutdown 02:00
```

```bash
sudo shutdown +20
```
```bash
sudo shutdown now
```

```bash
sudo syestemctl reboot
```
```bash
sudo syestemctl poweroff
```

```bash
 sudo wall 'System going into maintenance mode in 5 minutes!'
```

# Reference:
LPI - [learning material](https://learning.lpi.org/vi/learning-materials/101-500/101/101.3/101.3_01/) (in Vietnamese)
