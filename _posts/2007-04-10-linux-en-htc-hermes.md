---
layout: post
title: Linux en HTC Hermes
date: 2007-04-10 00:11:04.000000000 +02:00
type: post
categories:
- gadgets
- linux
tags: []
meta:
  tags: linux htc hermes
permalink: "/2007/04/10/linux-en-htc-hermes/"
---
Per fi, htc-hermes pot correr linux:

```
/ # uname -a Linux hermes 2.6.20-hh6 #66 Sat Apr 7 14:12:59 EDT 2007 armv4tl unknown / # cat /proc/cpuinfo Processor : ARM920T rev 0 (v4l) BogoMIPS : 199.47 Features : swp half thumb CPU implementer : 0x41 CPU architecture: 4T CPU variant : 0x1 CPU part : 0x920 CPU revision : 0 Cache type : write-back Cache clean : cp15 c7 ops Cache lockdown : format A Cache format : Harvard I size : 16384 I assoc : 64 I line length : 32 I sets : 8 D size : 16384 D assoc : 64 D line length : 32 D sets : 8 Hardware : HTC Hermes Revision : 0000 Serial : 0000000000000000
```

Passos per crear una imatge del kernel desde zero:

```
$ export CVSROOT=:pserver:anoncvs@cvs.handhelds.org:/cvs $ cvs login $ cvs co linux $ emerge crossdev $ crossdev -t arm $ make htchermes\_defconfig
```

Passos per utilitzar la imatge de kevin2:

[haret](http://www.handhelds.org/~koconnor/HTCHermes/herm-linux-20070407.exe) + [zImage](http://handhelds.org/~koconnor/HTCHermes/zImage) + default.txt:

`
set KERNEL zImage
bootlinux
`

Més informació:

- [hh.org htchermes kernel](http://handhelds.org/cgi-bin/cvsweb.cgi/linux/kernel26/arch/arm/mach-s3c2410/htchermes/)
- [htc hermes linux port @ xda-dev wiki](http://wiki.xda-developers.com/index.php?pagename=Hermes_Linux)
- [htc hermes linux port @ xda-dev forum](http://forum.xda-developers.com/viewtopic.php?p=338459)

Gràcies a cr2 i a kevin2!

Enjoy! :D

