---
layout: post
title: Mounting a VirtualBox dynamic size VDI Image
date: 2009-03-28 14:28:13.000000000 +01:00
type: post
categories:
- linux
tags:
- vdi
- virtualbox
meta:
permalink: "/2009/03/28/mounting-a-virtualbox-dynamic-vdi-image/"
---
> To work with a variable sized VDI file, you have to dump it to a dd file first (COPYDD); this will create a fixed size dump that then can be opened on a loop device. The variable sized file does not allow mounting as the fs driver needs to see the expected disk size.

So we proceed as follows:

```
~/.VirtualBox/VDI$ vditool COPYDD win.vdi dump vditool Copyright (c) 2008 Sun Microsystems, Inc.
```

Then we look where the partition starts, in my case this was a WinXP fat32 formatted filesystem:

```
~/.VirtualBox/VDI$ hexdump -C -v dump |head -n 30000 |grep -i dos 00007e00 eb 58 90 4d 53 44 4f 53 35 2e 30 00 02 10 24 00 |.X.MSDOS5.0...$.| 00008a00 eb 58 90 4d 53 44 4f 53 35 2e 30 00 02 10 24 00 |.X.MSDOS5.0...$.| ~/.VirtualBox/VDI$
```

Then we mount it:

```
~/.VirtualBox/VDI$ sudo mount -o loop,offset=0x7e00,umask=000 -o ro dump /mnt/
```
