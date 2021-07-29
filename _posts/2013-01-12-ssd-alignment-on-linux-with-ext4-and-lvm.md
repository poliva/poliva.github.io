---
layout: post
title: SSD alignment on linux with ext4 and LVM
date: 2013-01-12 01:08:36.000000000 +01:00
type: post
categories:
- linux
tags:
- align
- ext4
- fdisk
- lvm
- ssd
- trim
meta:
permalink: "/2013/01/12/ssd-alignment-on-linux-with-ext4-and-lvm/"
---
First make sure to create partitions aligned to your SSD erase block size (in my case 512k):  
`sudo fdisk -H32 -S32 /dev/sdb`  
You can check with `fdisk -lu /dev/sdb` that the start of each partition is divisible by 512.

Then initialize the desired partition to use with LVM2 using the <tt>dataalignment</tt> parameter:  
`pvcreate --dataalignment 512k /dev/sdb1`  
Make sure your `/etc/lvm/lvm.conf` contains the following options:

```
md_chunk_alignment = 1
data_alignment_detection = 1
data_alignment = 0
data_alignment_offset_detection = 1
```

Now you can use `vgcreate` to create your volume grup, and then `lvcreate` to create the logical volumes.

When creating ext4 filesystems (with TRIM support), use the following command:  
`mkfs.ext4 -O extent -b 4096 -E stride=128,stripe-width=128 /dev/mapper/vg1-test`  
<tt>stride</tt> and <tt>stripe-width</tt> are calculated as `sector size / block size` = 512k / 4k = 128

When mounting ext4 filesystems, use the '<tt>discard</tt>' parameter to enable TRIM support:  
`mount -o discard,noatime,nodiratime /dev/mapper/vg1-test /mnt/`

**Extra tip** : for more speed you can consider turning off journaling (to avoid double-write overhead), at the cost of an easily corruptable filesystem.  
Check if journaling is enabled: `dumpe2fs /dev/mapper/vg1-test |grep 'Filesystem features'`  
Disable journaling: `tune2fs -O ^has_journal`

