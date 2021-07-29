---
layout: post
title: Ubuntu Netbook Remix on HTC Shift
date: 2008-06-17 03:44:18.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: htc shift htc+shift UNR netbook+remix ubuntu netbook remix
permalink: "/2008/06/17/ubuntu-netbook-remix-on-htc-shift/"
---
kornel has [requested](/blog/2008/04/14/linux-on-htc-shift/#comment-154770) to post some instructions on installing [ubuntu netbook remix](https://launchpad.net/netbook-remix) on HTC Shift.

First, for those who still don't know what UNR is, a picture is worth 1000 words:

| [![ume-launcher on HTC Shift 1]({{ site.baseurl }}/assets/images/2008/06/shift-netbook-remix-01.png)](/photos/albums/HTC_Shift_Ubuntu/shift-netbook-remix-01.png) | [![ume-launcher on HTC Shift 2]({{ site.baseurl }}/assets/images/2008/06/shift-netbook-remix-02.png)](/photos/albums/HTC_Shift_Ubuntu/shift-netbook-remix-02.png) |

Here are the instructions:

1) Go to System -\> Administration -\> Software Sources. Click on Third Party software. Click Add..., and add the following:

```
deb http://ppa.launchpad.net/netbook-remix-team/ubuntu hardy main deb-src http://ppa.launchpad.net/netbook-remix-team/ubuntu hardy main
```

Then Click Close and Click Reload.

Advanced users can just add the above lines to their `/etc/apt/sources.list` (and run sudo apt-get update afterwards).

2) Install all netbook remix packages:

```
$ sudo apt-get install go-home-applet window-picker-applet maximus human-netbook-theme ume-launcher
```

'

3) Once installed, you'll need to add _maximus_ to autostart for your session. You'll also need to setup the gnome-panel to look like the screenshots, basically:

- Delete bottom panel
- Setup top panel like: GoHomeApplet|WindowPickerApplet|NotificationArea|MixerApplet|Clock

UPDATE: This has been fixed in current ume-launcher version.

~~You'll notice that the icons are very small by default, this is because UNR doesn't support resolutions below 1024x600. I have done a patch to ume-launcher which increases the icon size, available in [launchpad bug #237373](https://bugs.launchpad.net/netbook-remix-launcher/+bug/237373).~~

~~So, if you prefer bigger icons, just download my patched [ume-launcher\_0.3ubuntu3\_i386.deb](/HTC/shift/ume-launcher/ume-launcher_0.3ubuntu3_i386.deb), and install it.~~

