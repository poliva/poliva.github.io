---
layout: post
title: Actualitzar la bios de sistemes Dell desde linux
date: 2006-03-30 08:12:47.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: linux dell bios update dell_rbu
permalink: "/2006/03/30/actualitzar-la-bios-de-sistemes-dell-desde-linux/"
---
![Dell Linux]({{ site.baseurl }}/assets/images/2006/03/dell-linux.jpg)

Avui he vist que és posible fer un upgrade de bios als ordinadors de marca Dell (servers/desktop/laptop) desde Linux, hi ha la opció <tt>CONFIG_DELL_RBU</tt> al apartat _Firmware drivers_ als nous kernels (jo ho he vist a la 2.6.16), la documentació està en el fitxer `Documentation/dell_rbu.txt` de les fonts del kernel.

Més informació:

- [Dell linux blog: Dell BIOS Update in Linux](http://linux.dell.com/blog/2005/10/11/)
- [Extreure fitxers .HDR (bios header) dels .EXE](http://linux.dell.com/libsmbios/main/bios_hdr.html)
- [Post a Gentoo Forums](http://forums.gentoo.org/viewtopic-t-444247.html)

Si tinc temps intentaré actualitzar la bios del meu Latitude X300 amb aquest mètode!

