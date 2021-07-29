---
layout: post
title: 'hsect: HTC Shift Embedded Controller Toolkit'
date: 2008-05-31 05:20:17.000000000 +02:00
type: post
categories:
- linux
- wireless
tags: []
meta:
  tags: htc shift htc+shift wifi bluetooth bootloader hsect
permalink: "/2008/05/31/hsect-htc-shift-embedded-controller-toolkit/"
---
![HTC Shift Embedded Controller Toolkit]({{ site.baseurl }}/assets/images/2008/05/hsect2.png)  
I am proud to release **hsect** , the HTC Shift Embedded Controller Toolkit :D

Features:

- Turn on and off WLAN
- Turn on and off Bluetooth
- Enter and exit CE (ARM) Bootloader mode (SPL)
- Enter Radio Bootloader / OEMSBL\*
- Reboot Windows CE
- Switch To CE / SnapVUE
- Change the LEDs\*

\* features only available via command line

This means you don't need to boot Vista anymore to enable Wifi or Bluetooth after removing the battery, now you can do it on Linux or WinXP :)

### Download binary package

Ubuntu / debian package: [hsect\_2.1-1\_i386.deb](/HTC/shift/hsect_2.1-1_i386.deb)  
Windows cygwin executable (no GTK support): [hsect-2.1win32.zip](/HTC/shift/hsect-2.1win32.zip)

### Download source

[hsect-2.1.tar.gz](/HTC/shift/hsect-2.1.tar.gz)  
[hsect-2.0.tar.gz](/HTC/shift/hsect-2.0.tar.gz)  
[hsect-1.0.tar.gz](/HTC/shift/hsect-1.0.tar.gz)

Many thanks to Esteve Espuña who helped me with [Syser](http://www.sysersoft.com/products.html), the Vista kernel debugger we used to understand how the EC Controller driver (ecdrv.sys) works, and to cmonex who told me the DeviceIoControl she uses in her windows program to enter and leave bootloader mode and reboot CE, which translate directly to the EC commands used in this tool.

