---
layout: post
title: 'HTCFlasher: Open Source ROM Update Utility (RUU) for HTC devices'
date: 2008-06-14 11:28:24.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: htc flasher HTCFlasher RUU flash ROM upgrade linux
permalink: "/2008/06/14/htcflasher-open-source-rom-update-utility-ruu-for-htc-devices/"
---
I have just released HTCFlasher version 3, get it while it's hot!! :)

HTCFlasher -formerly known as [HERMflasher](http://forum.xda-developers.com/showthread.php?t=296436)- is an open source tool which allows you to flash ROMs on most current HTC devices. It has some extra features that the original HTC RUU doesn't have, like for example it can present a serial prompt to the bootloader (replacing mtty), or it can dump NBH file contents (.nb ROM parts).

Currently most new HTC devices are supported, and the basic set of functions to work with every HTC bootloader has been implemented, so adding support for new bootloader versions or new devices should be quite easy to do, if not working out of the box.

For an incomplete list, see [SupportedDevices](http://code.google.com/p/htc-flasher/wiki/SupportedDevices).

**Supported Operating Systems**

- GNU/Linux x86 and x86\_64
- Win32/Cygwin (except Vista)
- Mac OS X (intel based)

**Features**

- Flash NBH files: replaces the Windows Rom Upgrade Utility (RUU)
- Extract NBH files: replaces windows tools like nbhextract
- Serial prompt: replaces mtty / minicom
- Easy to use Gtk GUI

**Screenshots**

Main Window  
 ![HTCFlasher main window]({{ site.baseurl }}/assets/images/2008/06/Screenshot-HTCFlasher-1.png)

Flash NBH file  
 ![HTCFlasher flash NBH file]({{ site.baseurl }}/assets/images/2008/06/Screenshot-HTCFlasher.png)

For more information:

- [HTCFlasher Project Page](http://htc-flasher.googlecode.com/)
- [HTCFlasher WIKI](http://code.google.com/p/htc-flasher/wiki/HomePage)
- [Download](http://code.google.com/p/htc-flasher/downloads/list)
