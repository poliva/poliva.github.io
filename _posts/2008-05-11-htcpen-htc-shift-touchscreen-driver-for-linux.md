---
layout: post
title: 'htcpen: HTC Shift Touchscreen Driver for Linux'
date: 2008-05-11 19:37:27.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: htc shift htc+shift touchscreen htcpen linux kernel module
permalink: "/2008/05/11/htcpen-htc-shift-touchscreen-driver-for-linux/"
---
Here's my first attempt to make the HTC Shift touchscreen working under Linux :D

Feel free to send me patches and ideas to improve the driver :)

**Update** : htcpen submitted to [review for inclusion into Linux kernel](http://lkml.org/lkml/2008/5/27/199).  
**Update2** : htcpen has been included into the [git input tree](http://git.kernel.org/?p=linux/kernel/git/dtor/input.git;a=commit;h=5a18c343a6bee4b38965f14a40ccb95306641f87), it will be merged when 2.6.27 opens up.

### Download

htcpen version 1.6 (2008-05-28)  
[htcpen-1.6.tar.gz](/HTC/shift/touchscreen/htcpen-1.6.tar.gz)  
eGalax Xorg driver + TouchKit version 2.03  
[TouchKit-2.03.172.tar.gz](/HTC/shift/touchscreen/TouchKit-2.03.172.tar.gz)

### Install instructions

Download the tarball, extract it to some temporary directory, compile and install it:

```
# tar zxvf htcpen-1.6.tar.gz # cd htcpen-1.6 # make # make install
```

X.org configuration:

```
# tar zxvf TouchKit-2.03.172.tar.gz # cd TouchKit # cp egalax\_drv.so /usr/lib/xorg/modules/input/
```

Edit /etc/X11/xorg.conf and configure the following:

```
Section "InputDevice" Identifier "htcpen" Driver "egalax" Option "Device" "/dev/input/event\_htcpen" Option "Parameters" "/var/lib/egalax.cal" Option "ScreenNo" "0" Endsection
```

Into your "ServerLayout" section, add the following line:

```
InputDevice "htcpen" "CorePointer"
```

After rebooting your system, udev should load the htcpen module automatically and create a symlink of the input device to `/dev/input/event_htcpen`.

To finish, run "TouchKit", click on "Tool" tab and do the 25 point calibration.

<!--more-->

### htcpen demonstration video
