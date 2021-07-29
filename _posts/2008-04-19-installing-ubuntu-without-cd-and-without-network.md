---
layout: post
title: Installing Ubuntu without CD and without Network
date: 2008-04-19 00:27:46.000000000 +02:00
type: post
categories:
- gadgets
- linux
tags: []
meta:
  tags: ubuntu htc shift htc+shift mobile linux umpc
permalink: "/2008/04/19/installing-ubuntu-without-cd-and-without-network/"
---
Previously I used [Wubi-installer](http://wubi-installer.org/) to install Ubuntu 8.04 on my HTC Shift, but this time I wanted to partition my Hard Drive and use separate partitions for Ubuntu. Here's what I did...

First, resize the Vista partition: You don't need Partition Magic or gparted to do this, vista itself can do it. Right click on my computer, select Manage, click on Disk Management, Right click on the partition you want to shrink.

Then create 2 (or more) partitions using the Vista Computer Management console, one should be around 1Gb, which we will use to place the Ubuntu ISO image. Later on, when installation is finished you can use it as a swap partition. In the HTC Shift you must create these two partitions using Vista Disk Management tool, if you do it with Linux you can easily damage the Host Protected Area partition which holds the Vista Recovery (Fn + F3 at boot time), so it is safer to do this with Vista.

Now install [Slax on a USB Pendrive](http://www.pendrivelinux.com/2006/09/20/all-in-one-usb-slaxzip/). When installed, copy the following files to the PenDrive too:

- [Slackware Grub package](ftp://ftp.slackware.com/pub/slackware/slackware-12.0/extra/grub)
- [Ubuntu Hardy initrd.gz and vmlinuz](http://archive.ubuntu.com/ubuntu/dists/hardy/main/installer-i386/current/images/hd-media/)

Now boot using the USB stick (on HTC Shift, Fn+F10 at boot time). Once Slax has booted, use `cfdisk` to setup your Linux partitions, remember you need to leave a ~1Gb ext3 partition for the ISO.

Format the 1Gb partition as Ext3 (change _XX_ to your partition number):

```
mke2fs -I 128 -j /dev/hdaXX
```

the '_-I 128_' is to set the inode-size to 128 as grub can't read bigger inode sizes, and Slax formats to 256 by default.

Mount the partition in /boot:

```
# mount /dev/hdaXX /boot
```

Install grub package on Slax:

```
# cd /mnt/sda1 # installpkg grub-0.97-i486-3.tgz
```

Copy the files needed to boot Ubuntu installer on the 1Gb partition:

```
# cp ubuntu-8.04-alternate-i386.iso vmlinuz initrd.gz /boot # cd /boot ; ln -s . boot
```

Create the 'menu.lst' file for grub:

```
title Install Ubuntu root (hdX,Y) kernel /boot/vmlinuz vga=normal ramdisk\_size=14972 root=/dev/rd/0 rw -- initrd /boot/initrd.gz
```

hdX,Y should be changed to the value matching your partition, in my case it was (hd0,2) which refers to the 1st hard drive 'hd0' and the 3rd partition '2'.

Now Install grub:

```
# grub-install /dev/hda
```

Reboot, and ubuntu installation should start. Remember not to screw the 1Gb partition during the installation, you can safely remove it once ubuntu has been installed.

