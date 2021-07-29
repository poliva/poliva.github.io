---
layout: post
title: How to dump and restore the Vista recovery partition on HTC Shift
date: 2008-04-22 01:45:57.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: htc+shift linux partitioning HPA vista recovery boot
permalink: "/2008/04/22/how-to-dump-and-restore-the-vista-recovery-partition-on-htc-shift/"
---
The HTC Shift HDD is 40 GB, exactly 40000536576 bytes. The [Host Protected Area](http://en.wikipedia.org/wiki/Host_Protected_Area) starts at 0x88FE00000 and is exactly 3GiB, from linux dmesg output:

```
sda: Host Protected Area detected. current capacity is 71826615 sectors (36775 MB) native capacity is 78126048 sectors (40000 MB) sda: Host Protected Area disabled. sda: 78126048 sectors (40000 MB), CHS=16383/255/63
```

Here's the [radare](http://radare.nopcode.org) dump, showing the end of the Vista partition and the start of the Host Protected Area, where the Shift HDD stores the Vista recovery information.  
 ![]({{ site.baseurl }}/assets/images/2008/04/shift-radare.png)

Here is how gparted sees the HTC Shift partitions, the 3.00 GiB "unallocated" space at the end holds the vista recovery information, you can't see this space in Vista.

![]({{ site.baseurl }}/assets/images/2008/04/shift-gparted.png)

So we can dump it using 'dd': 0x88FE00000 == 36773560320 , we will read & write 16384 bytes at a time, to speed up the process, so 36773560320/16384 = 2244480

```
# dd if=/dev/sda of=/media/disk/shift-vista-recovery.bin bs=16384 skip=2244480
```

If we keep the bin file in a safe place, we can happily use the unallocated space and gain 3GiB of space in our HDD.

The [md5sum](http://en.wikipedia.org/wiki/Md5sum) of my Spanish Vista is the following:

```
# md5sum shift-vista-recovery-es.bin 5c3a9ea3ea578419daf3f1f242755122 shift-vista-recovery-es.bin
```

If later on we need to restore it, to be able to recover vista using the Fn+F3 key combo at boot time, we must place it in the same place so, using 'dd' this would be:

```
dd if=/media/disk/shift-vista-recovery.bin of=/dev/sda bs=16384 seek=2244480
```

You can always boot from a USB pendrive using [Slax](http://www.pendrivelinux.com/2006/09/20/all-in-one-usb-slaxzip/), to perform the 'dd' operations.  
Note: Replace '/dev/sda' for '/dev/hda' if using Slax.

