---
layout: post
title: 'gsensor-joy: HTC Shift G-Sensor Joystick Linux kernel module'
date: 2008-06-05 21:18:43.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: htc shift htc+shift G-sensor gsensor LIS3LV02DL LIS3LV02DQ i2c SMBus gsensor-joy
    linux kernel module
permalink: "/2008/06/05/gsensor-joy-htc-shift-g-sensor-joystick-linux-kernel-module/"
---
I have found some free time (long night) to implement a kernel module which converts the axis reported by the HTC Shift [G-Sensor accelerometer (see previous post!)](/blog/2008/06/03/i2c-gsensor-lis3lv02dl-accelerometer-on-htc-shift-g-sensor/) into a Joystick input event device.

### Download

[gsensor-joy-1.1.tar.gz](/HTC/shift/gsensor/gsensor-joy-1.1.tar.gz)

### Install Instructions

Download the tarball, extract it to some temporary directory, compile and install it:

```
# make # make install
```

Load the needed modules:

```
#modprobe i2c\_i801 #modprobe i2c\_dev #modprobe gsensor-joy #modprobe joydev
```

If you want that automatically loaded every boot, you can also add them to `/etc/modules`.

For X.org configuration see the README file included in the tarball.

Here's a demo video playing TuxRacer...

<!--more-->

### Demo Video
