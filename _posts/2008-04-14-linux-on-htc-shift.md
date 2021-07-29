---
layout: post
title: Linux on HTC Shift
date: 2008-04-14 23:36:31.000000000 +02:00
type: post
categories:
- gadgets
- linux
tags: []
meta:
  tags: htc shift htc+shift ubuntu mobile linux umpc
permalink: "/2008/04/14/linux-on-htc-shift/"
---
I recently got an <acronym title="Ultra Mobile PC">UMPC</acronym> [HTC Shift](http://www.htc.com/shift) (also known as HTC P9500, HTC Shangrila or HTC Clio).

You can install Ubuntu Hardy 8.04 using [Wubi Installer](http://wubi-installer.org/), so you don't need to partition the Hard Drive. Installation is pretty straight forward, just choose the installation size (I would recommend no less than 7Gb) and click "next", "next"... If you prefer to install on a separate partition, follow [the instructions here](/blog/2008/04/19/installing-ubuntu-without-cd-and-without-network/).

Once Ubuntu is installed, you get a menu at boot time which lets you choose which operating system to boot, Vista or Ubuntu. Surprisingly most of the hardware is auto-detected by ubuntu, and almost everything works out of the box:

- Audio is working, mute and volume control works using the Fn keys.
- SD card reader is working.
- Adjusting the screen backlight works with the proper Fn keys.
- Webcam is working (you can test it with `gstreamer-properties`).
- CPU frequency scaling works by default too on the [Intel Stealy](https://wiki.ubuntu.com/McCaslin) 800Mhz CPU, you can monitor it by enabling the cpufreq gnome pannel.
- ACPI is working, you can get the CPU temperature using the sensors-applet.
- Screen resolution works at 800x480. I have not tried higher resolutions yet.
- Bluetooth is working.

However there are a few things that require some extra work in order to have them working properly. I'll walk through some of them in this post.

### Setting up HTC Shift "The Easy Way"<sup>TM</sup>

If you are running Ubuntu/Kubuntu/Xubuntu 8.04, I have created a script which will automatically setup the following:

- TouchScreen
- Wifi
- 3G Connectivity
- suspend/resume
- Hardware buttons support
- rotate screen utility
- Embedded Controller Toolkit (lets you enable/disable wifi and bluetooth)

You can download it from here:  
**[htcshift-easy-setup-v1.2.tar.bz2](/HTC/shift/htcshift-easy-setup-v1.2.tar.bz2)**

Install Instructions:

Put _htcshift-easy-setup-v1.2.tar.bz2_ in a USB memory stick, insert it in the Shift and Ubuntu will automatically mount it as `/media/disk/`. Then issue the following commands:

```
$ sudo tar jxvfp /media/disk/htcshift-easy-setup-v1.2.tar.bz2 $ cd htcshift-easy-setup
```

- Run '`sudo ./install.sh kernel`' for keeping the default usb-rndis kernel modules: If you choose this option, the provided USB ethernet will work, however you will not be able to use 3G connectivity or to transfer files from WinCE using synce.
- Run '`sudo ./install.sh synce`' for keeping the synce usb-rndis kernel modules: If you choose this option, you will be able to use 3G connectivity and to transfer files from WinCE using synce, however the provided USB ethernet will not work.

After installation, the two right side hardware buttons will be mapped to launch [hsect2](/blog/2008/05/31/hsect-htc-shift-embedded-controller-toolkit/) (enable/disable wifi & bluetooth) and to rotate the screen, and you should have the following new items in Accessories menu:

- TouchKit (Touch Screen Calibration Utility)
- HTC Shift Embedded Controller Toolkit
- Rotate Screen

You can switch from kernel to synce or viceversa at any time, just run the install.sh again.

To use 3G, you need to run USBTool.exe on CE, select "Attach to Vista", then open the "Internet Sharing" application on CE and click "Connect". Switch to linux, you should get the network settings over DCHP on rndis0 interface. Ubuntu's NetworkManager sees it as a wired network.

Now I'll explain the "long dificult way"<sup>TM</sup>, for those who don't want to use my script or have a different kernel version or distribution.

### Wi-Fi: Marvell SD8686 Wireless Lan SDIO

The wlan card does not work by default, the driver for it is missing in ubuntu 8.04 beta. I have submitted a [bug report](https://bugs.launchpad.net/ubuntu/+source/linux-ubuntu-modules-2.6.24/+bug/210839), so hopefully it is included in the final 8.04 release, but at the moment you have to compile the driver yourself. Luckily for us, Marvell has published an open source driver plus a proprietary firmware which allows the wifi to work. The sources are available on the [linux-ubuntu-modules-2.6.24](http://packages.ubuntu.com/source/hardy/linux-ubuntu-modules-2.6.24) package.

To compile it, download the [linux-ubuntu-modules package source](http://archive.ubuntu.com/ubuntu/pool/main/l/linux-ubuntu-modules-2.6.24/), untar it and edit the file `ubuntu-hardy-lum/debian/config/i386` to add the following:

```
CONFIG\_MMC\_SD8686=m CONFIG\_MMC\_SD8688=m
```

Then compile it using '`dpkg-buildpackage`', the resulting module will be in `ubuntu-hardy-lum/debian/build/build-386/wireless/marvell/8686_wlan/sd8686.ko`, copy it to your `/lib/modules` and run `depmod -a`.

You also need to place the proprietary Marvell firmware to `/lib/firmware/mrvl/` directory, the firmware can be downloaded from Marvell's website [here](http://www.marvell.com/drivers/driverDisplay.do?dId=200&pId=38).

To get the wifi loaded automatically at boot time edit `/etc/modules` and add 'sd8686'.

### 3G/HSDPA connectivity

3G connectivity is achieved through the CE / SnapVUE side, the same way you do it in Vista, but using the _usb-rndis-lite_ linux module from SynCE. It just allows you to tether the "embedded" MSM7200 device in the Shift with the x86 side, using the USB connection that links them.

For it to work, you first need to get synce (which will also allow you to sync Ubuntu contacts and appointments with CE, share files between both systems, etc...) and the SVN version of the rndis-lite module:

```
$ sudo apt-get install synce-dccm synce-multisync-plugin synce-serial libsynce0 libsynce0-dev librra0 librra0-dev librra0-tools subversion build-essential $ svn co http://synce.svn.sf.net/svnroot/synce/trunk/usb-rndis-lite/ $ cd usb-rndis-lite/ $ make $ sudo ./clean.sh $ sudo make install
```

You can also follow the [Synce With Ubuntu instructions](http://www.synce.org/moin/SynceWithUbuntu) on SynCE website.

To enable 3G connectivity, you need to switch to CE, use the USBTool.exe utility to "Attach to Vista" (this will enable the USB connection between the PocketPC side and the PC side) and then enable "Internet Sharing" in WM6. Network settings can be acquired through DHCP on interface rndis0.

### Fingerprint Reader: AuthenTec AES1610

Ubuntu 8.04 does not have support for the AuthenTec AES1610, however this reader is supported in Linux using the [fprint library](http://reactivated.net/fprint/wiki/). Unfortunately the current version doesn't seem to work very well (it fails recognizing the fingerprint most of the times), but if you want to try it, there are some [precompiled ubuntu packages](http://www.madman2k.net/comments/105) in launchpad. To install them, add the following line to your `/etc/apt/sources.list`:

```
deb http://ppa.launchpad.net/madman2k/ubuntu hardy main restricted universe multiverse
```

Then install the libpam-fprint + libfprint + fprint-demo packages:

```
$ sudo apt-get install libpam-fprint libfprint0 fprint-demo
```

You can test the reader by running `gksudo /usr/bin/fprint_demo`.

### Touchscreen

Finally we have a working touchscreen, read instructions on this post:

[htcpen: HTC Shift Touchscreen Driver for Linux](/blog/2008/05/11/htcpen-htc-shift-touchscreen-driver-for-linux/)

### What still doesn't work

- **touch screen** : Touchscreen working with [htcpen driver](/blog/2008/05/11/htcpen-htc-shift-touchscreen-driver-for-linux/) :D ~~The touch screen is not working yet, AFAIK there is no driver for linux (nor for Windows XP). Vista uses a "HTC Touch Screen Driver, V1.0.0.2" which will need to be disassembled first. Meanwhile you'll have to use the Synaptics Touchpad, which does its job nicely.~~
- **suspend** : Suspend works, ~~but the device doesn't resume properly. I'll have to look what is preventing it to resume, and probably disable the offending device driver before suspending. Will post more details if I get it working.~~ download the [suspend / resume scripts for HTC Shift](/blog/2008/04/21/suspend-resume-in-htc-shift/) here :D

### Screenshots

To finish, a couple of screenshots, click on them to maximize.

| [![linux on HTC Shift 1]({{ site.baseurl }}/assets/images/2008/04/Screenshot01.png)](/photos/albums/HTC_Shift_Ubuntu/Screenshot01.png) | [![linux on HTC Shift 2]({{ site.baseurl }}/assets/images/2008/04/Screenshot02.png)](/photos/albums/HTC_Shift_Ubuntu/Screenshot02.png) | [![ume-launcher on HTC Shift 1]({{ site.baseurl }}/assets/images/2008/04/shift-netbook-remix-01.png)](/photos/albums/HTC_Shift_Ubuntu/shift-netbook-remix-01.png) | [![ume-launcher on HTC Shift 2]({{ site.baseurl }}/assets/images/2008/04/shift-netbook-remix-02.png)](/photos/albums/HTC_Shift_Ubuntu/shift-netbook-remix-02.png) |

