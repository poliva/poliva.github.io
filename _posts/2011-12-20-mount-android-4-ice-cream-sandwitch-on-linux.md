---
layout: post
title: Mount Android 4 Ice Cream Sandwitch on Linux
date: 2011-12-20 02:23:11.000000000 +01:00
type: post
categories:
- android
- linux
- minipost
tags:
- android
- filesystem
- fuse
- mount
- mtp
- ptp
- storage
- usb
meta:
  yourls_tweeted: '1'
permalink: "/2011/12/20/mount-android-4-ice-cream-sandwitch-on-linux/"
---
I recently got a Galaxy Nexus running Android 4.0 Ice Cream Sandwitch, which by default does not support USB Mass Storage. When connected to the computer via USB cable, we can choose to connect it as a Media device ([MTP](http://en.wikipedia.org/wiki/Media_Transfer_Protocol)), or as a Camera ([PTP](http://en.wikipedia.org/wiki/Picture_Transfer_Protocol)), as you can see in the picture below:

[![Android USB computer connection]({{ site.baseurl }}/assets/images/2011/12/android-usb-computer.png "Screenshot\_2011-12-20-02-45-59")](/files/2011/12/Screenshot_2011-12-20-02-45-59.png)

### Option 1: PTP

If we choose to mount it as a camera, PTP is integrated nicely into nautilus and you can browse the folders, however only pictures and video filetypes are supported.

### Option 2: MTP

If we want to use MTP on Linux, we need to install the package 'mtpfs' first, and then mount the device on the desired mountpoint, on Ubuntu you can just type:

```
$ sudo aptitude install mtpfs $ mkdir ~/android/ $ mount.mtpfs ~/android/
```

### Option 3: adbfs

The third option is [adbfs](http://collectskin.com/adbfs/), a fuse filesystem for adb. You can get the source on [github](https://github.com/isieo/adbFS). Once compiled, make sure "USB debugging" is enabled in android settings, and you can mount the device as follows:

```
$ adbfs /media/android/ -o modules=subdir -o subdir=/mnt/sdcard/
```

This option is slower than MTP, however all contents are accessible as it uses adb pull/push commands in the background to retrieve the file contents.

