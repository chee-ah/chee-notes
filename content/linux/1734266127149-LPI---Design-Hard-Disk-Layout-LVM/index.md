---
title: "LPI - Design Hard Disk Layout LVM"
date: 2024-12-15
draft: true
description: "Notes about LVM and Hard Disk Layouts"
series: ["LPI - Linux Essentials"]
series_order: 5
tags:
  - Linux
  - LPI
  - Linux Essentials
  - Study Notes
  - LVM
  - Disk
---

# Design Hard Disk Layout - LVM 
Managing and Configuring LVM (Logical Volume Manager) Storage in Linux
It is a great way to give us storage flexibility in Linux. Whether we have free space on different portions of the a disc or we have multiple discs and want to combine them together and represent to the operating system as one regular continuos partition. It also allows us flexibility with resizing those partitions, growing them almost infinitely if we keep adding storage space.
Most of the time LVMs tool will be installed on CentOS . If it is not pre-equipped with them , we can use:

```bash
sudo dnf install lvm2
```

## PV Physical volume : represent real storage devices that an Lvm will work with 

```bash
sudo lvmdiskscan
```
- To add two of our new  disc as physical volumes to LVM, we need to pass the block device file that point to these disc or partitions:

```bash
sudo pvcreate dev/sdc dev/sdd 
```
(just match with virtual machine --> could be vdc,vdd)...different letters) 
- To see what physical volumes are currently attached to LVM is sudo pvs:
```bash
sudo pvs
```

(In here, Psize will show the total size of PV, Pfree shows how much of this is still unpartitioned and not used biological volumes,)
- Next we need to tell it how to use this storage capacity -> add the PVs to a VG 

## VG Volume group:
-  VGcreate followed by the name of the group and the name of the physical volumes to use:

```bash
sudo vgcreate my_volume /dev/sdc  dev/sdd
```

In this case, we added two disks to our LVM that were sdc and sdd, they are two separate  5 gigabyte discs , once we add them to the volume group this is going to be seen as a single continuos 10 gigabyte disc. 
we can keep growing  our volume group indefinitely just by adding more disks into the server, then adding them into this volume group. It likes having a single disk that can keep growing and the server needs to store more data. This mean powering off the server, moving data around...normally very inconveniently but with volume group we can expand our storage without powering off the server.  
- To expand Volume group, we need to add new PV to the LVM. (create physical volume)

```bash
pv create /dev/sde
```
Then, use the command vgextend to add new PV to  the volume group.

```bash
sudo vgextend my_volume /dev/sde
```
To see Volume group:

```bash
sudo vgs
```
From here, if we want to remove a physical volume from a volume group , we use vgreduce:

```bash
sudo vgreduce my_volume /dev/sde
```
If we don't need that physical volume anymore, we can even remove that from the LVM entirely with pvremove:

```bash
sudo pvremove /dev/sde
```
We went from PVs to VGs, VG like a single disk in an LVM , so what's missing is this disc or VG is not partitioned yet, so we need to add a logical volume  

## LV Logical Volume
 - Create the first logical volume 
 
 ```bash
 sudo lvcreate --size 2G -- name partition1 my_volume
 ```
 - Look for information of LV

 ```bash
 sudo lvs
 ```
How data will be organized:

 Data in LVMs is divides int multiple PEs. Those are physical extents.
 For example, we have two incredibly small hard disks, four mags each. We add these two PBs to one eight-megabyte VG, and we create a logical volume of eight megs on this volume group. Now we write an eight -meg file. To an application, it looks lke it writes this file into a file system that exists on a single continuous disk. In reality, LVM has split this data into two physical extents. A four- megabyte physical extent will be written on the first hard disk,  and another four-megabyte physical extent will be written on the second hard disk. working with the multiple disks or multiple partitions works the same way. To tell LVM to expand the first logical volume to all the extents that it has available. Use a command aptly named lvresize:

 ```
 sudo lvresize --extents 100%VG my_volume/partition1
 ```
 If we want to shrink it, we use lvresize 

 ```bash
 sudo lvresize --size 2G myvolume/partition1
 ```

 It is going to give us a warning that it might destroy data ..
 An empty logical volumes like an empty partition.
 To be able to store files and directories on a logical volume, we need to create a file system on it. we know that partitions can be found at location like dev BDA where logical volume's found. The path to an LV is pretty intuitive as well, it's going to be/dev just like a partition. This is going to be the same of the volume group/ the name of the logical name. You can find those and the details about the PVs by using lvdisplay.
 
 ```bash
sudo lvdisplay
```

if we want to create an XFS file system with the default options

```bash
sudo mkfs.xfs /dev/my_volume/partition1
```

Now we have an XFS file system on our first logical volume.

If we have a file system on it, we need to be a little more careful. if we want to resize a logical volume with a file system on it, we should not use a command like that.

```bash
sudo lvresize --size 3G myvolume/partition1
```
This would only resize the logical volume from two gigabytes to three gigabytes. 
we can tell LVM to resize both the logical volume and the file system on it by passing. That XFS file system we just put on it would still only use two gigabytes. we can tell LVM to resize both the logical volume and the file system on it passing another parameter . That's going to be resizefs 

```bash
sudo lvresize --resizefs  --size 3G my_volume/partition1
```
When you are doing this is that some file systems can be enlarged, they can be grown in size but they can't shrink 


PE Physical Extend

If we forget any command that we have been using 
```bash
man lvm
```
