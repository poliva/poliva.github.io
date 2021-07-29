---
layout: post
title: 'i2c-gsensor: LIS3LV02DL accelerometer on HTC Shift G-Sensor'
date: 2008-06-03 03:04:40.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: htc shift htc+shift G-sensor gsensor LIS3LV02DL LIS3LV02DQ i2c SMBus
permalink: "/2008/06/03/i2c-gsensor-lis3lv02dl-accelerometer-on-htc-shift-g-sensor/"
---
The "feature" commercially advertised as G-Sensor in HTC Shift turned out to be a [STMicroelectronics LIS3LV02DL 3-axis inertial sensor](http://www.st.com/stonline/products/literature/ds/12094/lis3lv02dl.htm) [[datasheet](http://www.st.com/stonline/products/literature/ds/12094.pdf)], which of course can be accessed through linux i2c/SMBus interface :)

Actually there's a Linux kernel module for the HP Mobile Data Protection System 3D (mdps) by Yan Burman, which is the same accelerometer used in some HP laptops, however those laptops have the accelerometer connected through an SPI interface instead of i2c, so the driver is not suitable for the HTC Shift as it is, but maybe in the future... ;)

As a proof of concept, here's an small userland tool that will show the output of X,Y,Z axis when moving the HTC Shift, just as if it was the Wii Remote :) To use it you need to modprobe _i2c\_i801_ and _i2c\_dev_ modules first.

Download: **[i2c-gsensor.tar.gz](/HTC/shift/i2c-gsensor.tar.gz)**.

**Update** (05/06/2008): [**gsensor-joy** : HTC Shift G-Sensor Joystick Linux kernel module](/blog/2008/06/05/gsensor-joy-htc-shift-g-sensor-joystick-linux-kernel-module/)

