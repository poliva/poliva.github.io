---
layout: post
title: Suspend / resume in HTC Shift
date: 2008-04-21 00:17:39.000000000 +02:00
type: post
categories:
- gadgets
- linux
tags: []
meta:
  tags: suspend resume hibernate htc+shift htc shift linux pm-utils
permalink: "/2008/04/21/suspend-resume-in-htc-shift/"
---
After a dozen of reboots and hangs trying to figure out what was preventing the suspend / resume to work on HTC shift I finally managed to get it working (both, suspend to ram and hibernate are working :D)

The pm-utils offer a nice interface for hooking your own scripts which are called during the suspend/hibernate and wake up events, so just removing the bogus modules and doing a bit of magic with dbus messages to stop and start wifi in NetworkManager the Shift is suspending properly in a matter of seconds and coming back to live even faster than Vista.

Download the suspend scripts for HTC Shift here, tested on Ubuntu 8.04:

- [suspend-shift-v1.tgz](/archives/files/suspend-shift-v1.tgz)

To install:

```
$ sudo tar zxvfp suspend-shift-v1.tgz -C /
```

Enjoy ;)

