---
layout: post
title: Linux on ONKYO BX407A4
date: 2010-07-30 14:31:43.000000000 +02:00
type: post
categories:
- gadgets
- linux
tags:
- BX407A4
- egalax
- onkyo
- opengalax
meta:
permalink: "/2010/07/30/linux-on-onkyo-bx407a4/"
---
On a recent trip to Japan I bought this UMPC, of course I was not happy with the default Japanese windows OS, and changed it to Ubuntu. Here are some quick notes I took during the process for me to remember if I need to reinstall in the future and hoping it can be useful to anyone else having this UMPC. As far as I know, this is the same hardware as UMID Mbook BZ / M2 or Sagem SPIGA, so instructions should be valid for these devices as well.

![onkyo bx407a4]({{ site.baseurl }}/assets/images/2010/07/onkyo-bx4.jpg)

<!--more-->

### Backup of the recovery partition

Boot with a USB bootable linux distro, and backup the whole sda1 partition, it contains a FreeDOS with a Symantec Ghost of the default Windows installation (in Japanese), keep it in a safe place in case you need to restore it in the future:

```
dd if=/dev/sda1 bs=16384 |gzip -9 \> /mnt/onkyo-sda1.tgz
```

### Ubuntu installation

I installed the Netbook edition of Ubuntu 10.4, you can install it using a USB pen drive. To select the boot device press Fn+F11 during boot (you can enter the BIOS pressing Fn+Del while booting).

### Fix Wireless: Libertas SDIO SD8686

The default module in ubuntu 2.6.32-21-generic kernel needs to be patched, otherwise the system hangs when associating to an access point. I got the [patch](/archives/libertas.patch) and instructions from [this website](http://nblog.jp/0132) (in Japanese!). In short, this is what needs to be done (you can do it on a virtual machine if you don't want to make the system dirty just to get the two patched .ko files for your kernel):

```
$ sudo apt-get install build-essential kernel-package linux-source-2.6.32 git-core $ cd /usr/src $ sudo tar xvjf linux-source-2.6.32.tar.bz2 $ cd linux-source-2.6.32 $ sudo cp /boot/config-2.6.32-21-generic .config $ sudo make oldconfig $ sudo git apply libertas.patch $ sudo cp /usr/src/linux-headers-2.6.32-21-generic/Module.symvers . $ sudo make modules\_prepare $ sudo make M=drivers/net/wireless/libertas
```

This will create libertas.ko and libertas\_sdio.ko which needs to be replaced in /lib/modules/.

To get the wifi working, you also need to place the libertas firmware [downloaded from marvell website](http://extranet.marvell.com/drivers/driverDisplay.do?driverId=203) in /lib/firmware, this is important as the firmware provided by default in Ubuntu (v8 and v9) doesn't work:

```
# unzip SD-8686-LINUX26-SYSKT-9.70.3.p24-26409.P45-GPL.zip # tar xvfp SD-8686-FEDORA26FC6-SYSKT-GPL-9.70.3.p24-26409.P45.tar # cp FwImage/sd8686.bin /lib/firmware/sd8686.bin # cp FwImage/helper\_sd.bin /lib/firmware/sd8686\_helper.bin
```

To enable IEEE PS (power management:on), add the following to your /etc/rc.local:

```
iwconfig wlan0 power on
```

### suspend/resume and hibernation:

Currently there is a known bug in Intel GMA500 'Poulsbo' where 3D Graphics (OpenGL) does not work after resume. Some threads in ubuntu forums suggest to use uswsusp (s2ram) instead of the default pm-suspend, and removing the package vbetool. I have tried all sorts of combinations and couldn't get resume to work: suspend works using the command 's2ram -f -a 2 -m', but after resuming you can't see the terminal nor the X display.

Hibernation works out of the box using the default pm-hibernate, however after the computer is back it goes to gdm login screen and doesn't allow the user to login, so at the moment this feature is completely useless.

### Fix Sound: snd-hda-intel Realtek ALC262

By default sound works, however the internal microphone does not record anything. To fix it add the following line to /etc/modprobe.d/alsa-base.conf:

```
options snd-hda-intel model=basic
```

Check the Capture settings in alsamixer (press F4 to see Capture settings). If you activate the Mic setting in the Playback screen (F3 or F5) then you get annoying whistling, leaving that setting to zero works fine.  
Another solution is to install an advanced graphical mixer: pavucontrol

To enable power saving on the soundcard, you can add the following to /etc/rc.local:

```
echo 1 \> /sys/module/snd\_hda\_intel/parameters/power\_save
```

### Fix Graphics: Intel GMA500

This will enable the screen resolution to 1024x600, check [ubuntu wiki](https://wiki.ubuntu.com/HardwareSupportComponentsVideoCardsPoulsbo) for more information.

```
add-apt-repository ppa:gma500/ppa apt-get update apt-get install poulsbo-driver-2d poulsbo-driver-3d poulsbo-config
```

Note, this step must be done before installing the TouchScreen driver.

If you are having problems not being able to login (due to a current bug in poulsbo drivers), it helps adding this to the ServerFlags section of your xorg.conf file:

```
Section "ServerFlags" Option "AIGLX" "off" EndSection
```

Also, you must change the Power Management settings to don't Reduce the backlight brightness nor Dim display when idle, otherwise the screen goes completely dark.

Xv video output doesn't work (Totem, webcam, skype, etc). Ubuntu wiki suggest to use mplayer-vaapi to get decent video playback, but I found [VLC](http://ubuntuforums.org/showpost.php?p=9598669&postcount=1461) to be a better solution. Just follow these instructions for video playback:

```
sudo add-apt-repository ppa:nvidia-vdpau/cutting-edge-multimedia sudo apt-get update sudo apt-get install vlc mplayer
```

Then open VLC media player, go to Tools, Preferences, Video and select "X11 Video Output (XCB)" as video Output.  
For video playback with mplayer, use: mplayer -vo vaapi -va vaapi videofile.

### Fix TouchScreen

First, add the following to /etc/rc.local:

```
echo -n serio\_raw\>/sys/bus/serio/devices/serio4/drvctl
```

Then download the latest [eGalax Touch Driver](http://home.eeti.com.tw/web20/eGalaxTouchDriver/linuxDriver.htm), untar it and run setup.sh as root. You need to select "2" (PS/2 device). After running setup, reboot the computer and run eGalaxTouch to calibrate the touchscreen.

### Fix Hotkeys

Create the file /lib/udev/keymaps/onkyo-bx4 with this content:

```
0xE2 cyclewindows # Fn+Esc 0xEE battery # Fn+Q 0xDF sleep # Fn+W 0xF5 switchvideomode # Fn+E 0xF0 record # Fn+R 0xF6 camera # Fn+T 0xF7 f22 # Fn+Y (touchpad toggle) 0xF9 brightnessdown # Fn+A 0xF8 brightnessup # Fn+S 0xA0 mute # Fn+D 0xAE volumedown # Fn+F 0xB0 volumeup # Fn+G 0xE0 bluetooth # Fn+H 0xFB wlan # Fn+J
```

Then, edit /lib/udev/rules.d/95-keymap.rules and add the following line inside the keyboard\_vendorcheck label:

```
ENV{DMI\_VENDOR}=="ONKYO CORPORATION", ATTR{[dmi/id]product\_name}=="ONKYOPC", RUN+="keymap $name onkyo-bx4"
```

Reboot and customize the hotkeys in gnome-keybinding-properties, I have added the following Custom Shorcuts:  
1) Toggle wifi (XF86WLAN): make it run the following script

```
#!/bin/bash ifconfig |grep ^wlan if [$? == 0]; then dbus-send --system --type=method\_call --dest=org.freedesktop.NetworkManager /org/freedesktop/NetworkManager org.freedesktop.DBus.Properties.Set string:org.freedesktop.NetworkManager string:WirelessEnabled variant:boolean:false else dbus-send --system --type=method\_call --dest=org.freedesktop.NetworkManager /org/freedesktop/NetworkManager org.freedesktop.DBus.Properties.Set string:org.freedesktop.NetworkManager string:WirelessEnabled variant:boolean:true fi
```

2) Toggle bluetooth (XF86Bluetooth): make it run the following script

```
#!/bin/bash rfkill list |grep "blocked: yes" if [$? == 0]; then rfkill unblock bluetooth else rfkill block bluetooth fi
```

3) Camera App (XF86WebCam): make it run 'cheese' application  
4) Sound Recorder (XF86AudioRecord): make it run 'gnome-sound-recorder'

The rest of the hotkeys work by default, without needing to add them in gnome-keybinding-properties.

### Fix Backlight issues

Launch gconf-editor, go to /apps/gnome-power-manager/backlight and set the following settings:

battery\_reduce = yes  
brightness\_ac = 100  
brightness\_dim\_battery = 20  
dpms\_method\_ac = off  
dpms\_method\_battery = off  
enable = yes  
idle\_brightness = 84  
idle\_dim\_ac = yes  
idle\_dim\_battery = yes  
idle\_dim\_time = 1

Then, make sure /apps/gnome-power-manager/general/use\_time\_for\_policy is disabled (off).

### Configuration tweaks for SSD

1) Disable file access time updates to reduce writes: edit /etc/fstab and add the options <tt>noatime,nodiratime</tt> to your linux partitions.  
2) Create a RAM disk mounted at “/tmp” to avoid temporary files SSD I/O by adding the following line to /etc/fstab: <tt>tmpfs /tmp tmpfs mode=1777</tt>  
3) Change the default I/O scheduler. [noop](http://en.wikipedia.org/wiki/Noop_scheduler) or [deadline](http://en.wikipedia.org/wiki/Deadline_scheduler) are better suited to SSD drives. In my tests I found deadline to be slightly better for my needs. To enable it:  
- Add <tt>elevator=deadline</tt> at the end of GRUB\_CMDLINE\_LINUX\_DEFAULT in /etc/default/grub.  
- run sudo update-grub  
- Adjust the "fifo\_batch" value for the deadline scheduler. A higher value should reduce seeks, but also increase latency. SSDs don't need to worry about excessive seeks, so we can set the value low. Add the following line to /etc/rc.local:

```
echo 1 \> /sys/block/sda/queue/iosched/fifo\_batch
```

4) Avoid writes to swap, add the following to /etc/sysctl.d/99-swap-on-ssd.conf:

```
vm.swappiness=1 vm.vfs\_cache\_pressure=50
```

5) Disable rsyslog if you don't need any logging on your netbook (YMMV):

```
sudo mv /etc/init/rsyslog.conf /etc/init/rsyslog.conf.noexec
```

### Other random things

Firefox extensions: [Grab-and-drag](https://addons.mozilla.org/en-US/firefox/addon/1250/), [meerkat](http://banshee.fm/~abock/meerkat/).  
Other browser: [midori](http://www.twotoasts.de/index.php?/pages/midori_summary.html), [chromium-browser](http://www.chromium.org/Home) (extension: [chromeTouch](https://chrome.google.com/extensions/detail/ncegfehgjifmmpnjaihnjpbpddjjebme)).  
Remember: On the time/date applet preferences, uncheck the box "Show date". Go to System-\>Preferences -\> Startup Applications, remove: Check for new hardware, evolution alarm, Gnome login sound, Print queue applet, Ubuntu One, User folders update, Visual Assistance.  
To enable USB autosuspend for non-input devices, add the following to /etc/rc.local:

```
echo auto \> /sys/bus/usb/devices/usb1/power/level echo auto \> /sys/bus/usb/devices/usb2/power/level echo auto \> /sys/bus/usb/devices/usb3/power/level echo auto \> /sys/bus/usb/devices/usb4/power/level echo auto \> /sys/bus/usb/devices/3-2/power/level
```

To load Intel Atom thermal sensor by default, add "coretemp" to /etc/modules.

### TODO

~~I'm still trying to figure out how to get the integrated optical micro touchpad working, if you have any hint on this please leave a comment. It looks like the touchscreen and the integrated optical micro-touchpad share the same i8042 port, which is used as a char device (like kernel 2.4.x does) assigned to serio0 by using the serio\_raw fix, and only dumps the touchscreen information.~~  
**Update 2012-07-02** : I've written [opengalax](https://github.com/poliva/opengalax), a touchscreen driver that allows to use the touchscreen and the optical micro touchpad together, for it to work you have to pass <tt>i8042.nomux=1</tt> and <tt>i8042.reset</tt> to your kernel parameters. No need to install closed source eGalax drivers anymore :)

