---
layout: post
title: 'Motorola Xoom: Review from an early adopter'
date: 2011-04-13 14:49:50.000000000 +02:00
type: post
categories:
- android
- gadgets
- linux
tags:
- android
- honeycomb
- motorola
- review
- tablet
- xoom
meta:
  yourls_tweeted: '1'
permalink: "/2011/04/13/motorola-xoom-review-from-an-early-adopter/"
---
![Motorola Xoom]({{ site.baseurl }}/assets/images/2011/04/xoom.jpg)  
Yesterday morning I received the wifi-only version of the 10.1-inch Android tablet Motorola Xoom. I bought it in USA because it wasn't yet available in Europe when I purchased it. I choose the wifi-only version because of the following main reasons:

- the 3G/4G version available from Verizon USA uses CDMA technology and is not compatible with European GSM carriers.
- I didn't want to wait more time to have an Android 3 tablet
- I don't want to pay for another 3G data connection, because I already have one in my phone's SIM card, which I can tether over wifi or bluetooth to give internet connectivity to the Xoom when there's no wi-fi available.

Out of the box, the device comes with Android Honeycomb version 3.0.1, and no support for microSD card yet (it is advised in the manuals that microSD card support will be available trough a future system update).

The Xoom ships with an "unlockable" bootloader, that means you can connect via fastboot and issue the command '<tt>fastboot oem unlock</tt>' to be able to flash unsigned code at the expense of voiding your warranty, and that's what you should do if you want support for microSD card today, because there's an unofficial kernel with microSD support already available: [Tiamat AOSP kernel Version 1.3.0](http://forum.xda-developers.com/showthread.php?t=978013).

<!--more-->

Another caveat is that it won't install the Youtube app available in the Android Market (when you try, it just won't install). This is easily solved by installing an [old youtube 2.1.6 APK](http://forum.xda-developers.com/showpost.php?p=12773203&postcount=27) and then rebooting, it will be automatically upgraded to the latest youtube version 3.0.15.

Flash support which wasn't available in the early days is now available by installing the Flash app through the official Android Market, no issues at all.

### Rooting the Motorola Xoom

There are many guides posted out there on how to root the Motorola Xoom automatically in one click... but I prefer to do it following the standard procedure and understand what I am doing rather than let some unknown app do whatever God knows with my device, so [here are the steps I followed](http://forum.xda-developers.com/showthread.php?t=1010568). On linux you don't need any weird motorola drivers, just the standard Android SDK tools (<tt>adb</tt> & <tt>fastboot</tt>) to perform the _oem unlock_ and flash the Tiamat kernel. Once done, just install the 'su' binary, 'superuser.apk' and busybox and you're done.

### Installing Ubuntu Linux on Motorola Xoom

It is possible to run Linux on the Motorola Xoom, it will not replace the android system but just use the android kernel and run the linux image on a chrooted environment. There's also the possibility to access the graphical X window system through VNC, although I don't want it, I just installed the minimal system to have the power of a full linux system shell. Instructions are available here: [Ubuntu Maverick on Motorola Xoom](http://forum.xda-developers.com/showthread.php?t=987740). I made [this script](/archives/files/linux-xoom.txt) to start and stop the linux chroot environment from command line in android terminal (must be placed in <tt>/system/bin/</tt>).

### Overall sensation

**The good:**

- Unlockable bootloader with fastboot support
- No crappy manufacturer UI customizations (no motorola bloatware on the default ROM), simple and plain android 3 user interface.
- No extra useless applications installed by default (well, only two silly games, handy to show the graphical power of Tegra2 to your friends)
- Battery life is decent, I drained it completely yesterday after playing intensively with it during 12 hours. Total charge time was less than 2 hours.
- 5 mpx rear camera with 2 flash leds is more than OK for my needs, but YMMV here.

**The bad:**

- It has a proprietary charger and it doesn't charge through the microUSB connector, I blame Motorola on this!
- It has a mini HDMI connector, but no HDMI cable included in the box

**The ugly:**

- Android Honeycomb runs very smoothly on the Xoom, even with all visual effects enabled, however some apps still don't have good landscape mode support or no big-screen support at all (you can still run them, but they look very ugly!).
