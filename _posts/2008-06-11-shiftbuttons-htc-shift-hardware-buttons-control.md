---
layout: post
title: 'shiftbuttons: HTC Shift Hardware Buttons control'
date: 2008-06-11 02:05:00.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: htc shift htc+shift shiftbuttons hardware buttons button daemon
permalink: "/2008/06/11/shiftbuttons-htc-shift-hardware-buttons-control/"
---
shiftbuttons is a program that monitors the two right side hardware buttons on HTC shift, and launches any desired program when a button is pressed.

Typical usage is to start it as daemon (-d option) when you start your X session, and map each button to the desired function. For example, you can go into Gnome menu System -\> Settings -\> Sessions. There you find a tab named 'Startup Programs', and add the following:

```
shiftbuttons -d -c "gksudo /usr/bin/hsect2" -r htcshift-rotate
```

This will launch hsect2 when you press the CommManager button, and will rotate the screen when you press the switch resolution button.

System wide configuration can be changed at `/etc/xdg/autostart/shiftbuttons.desktop`.

htcshift-rotate is also included in the tarball and the debian package.

### Download

**Source code** : [shiftbuttons-1.0.tar.gz](/HTC/shift/shiftbuttons/shiftbuttons-1.0.tar.gz)  
**Ubuntu / Debian package** : [shiftbuttons\_1.0-1\_i386.deb](/HTC/shift/shiftbuttons/shiftbuttons_1.0-1_i386.deb)

