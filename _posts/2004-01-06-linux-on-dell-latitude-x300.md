---
layout: post
title: Linux on Dell Latitude X300
date: 2004-01-06 19:31:58.000000000 +01:00
type: post
categories:
- gadgets
- linux
tags: []
meta:
  tags: ''
permalink: "/2004/01/06/linux-on-dell-latitude-x300/"
---
Here are some notes I took when I installed [Gentoo Linux](http://www.gentoo.org) in my laptop, in the hope that it will be useful for someone else. Some things explained here are specific for Dell Latitude X300 (or Samsung Q20, which has the same hardware), but it may be applicable to other laptop if it uses the same devices. Some tips here are specific for Gentoo as this is the distro I installed, maybe you want to install something else, then you'll have to figure  
out how to do it for your distro.

<!--more-->

* * *

### Kernel Choice

I did some tests with vanilla stock kernels, and with the -ac tree for 2.4 and -mm tree for 2.6. I don't usually use bleeding edge kernels in production system, but my tests showed that the kernel which gave me support for almost all hardware and features I have in my x300 is the latest -mm patch for the 2.6 kernel series; so that's what I recommend for this laptop.

The -mm tree is a collection of patches/fixes that maintains Andrew Morton (2.6 maintainer). Most of his patches are there for testing before they wind up in the next 2.6.x release.

#### RELATED FILES:

- You can get the latest -mm patch to the 2.6 kernel [here](http://www.kernel.org/pub/linux/kernel/people/akpm/patches/2.6/).
- My kernel [config file](/x300/kernel-config).

* * *

### Linux support for Pentium M processor

I use these variables in my `make.conf` file:

```
CHOST="i686-pc-linux-gnu" CFLAGS="-O3 -march=pentium3 -fprefetch-loop-arrays -funroll-loops -pipe" CXXFLAGS="${CFLAGS}"
```

If you are using gcc 3.4.x you can change CFLAGS to use `-march=pentium-m`, as it has support for it.

To control CPU clock scaling and frequency control I use [cpufreqd](http://sourceforge.net/projects/cpufreqd/) and [cpudyn](http://mnm.uib.es/~gallir/cpudyn/):

Clock scaling allows you to change the clock speed of the CPUs on the fly. This is a nice method to save battery power, because the lower the clock speed is, the less power the CPU consumes.

CPU Dynamic Frequency Control

Cpudyn is a user space program, that works on every processor supported by the kernel's cpufreq driver.  
By using cpudyn you should not not notice any performance impact, nevertheless you should save battery consumption and reduce the temperature of your laptop.

```
# emerge cpufreq # emerge cpudyn
```

#### RELATED FILES:

- [/etc/cpufreqd.conf](/x300/cpufreqd.conf)
- [/etc/conf.d/cpudyn](/x300/cpudyn)

* * *

### ACPI

Unfortunately the laptop's BIOS has a buggy DSDT (Differentiated System Description Table) which prevents ACPI from functioning correctly. The DSDT contains the Differentiated Definition Block, which supplies the information and configuration information about the base system. It is always inserted into the ACPI Namespace by the OS at boot time.

In order to get ACPI working in X300 the DSDT should be replaced with a fixed version, patching the kernel with acpi-custom-DSDT patch and supplying a custom DSDT.

This is the process to follow:

1) Check your BIOS version and [get  
the apropiate DSDT](http://acpi.sourceforge.net/dsdt/view.php?manufacturer=Dell&name=Latitude+X300) from [acpi.sourceforge.net](http://acpi.sourceforge.net). (Get a newer BIOS version from Dell if needed).

2) Get the latest sources of the -mm kernel branch:

```
# emerge mm-sources
```

3) Apply the [acpi-custom-DSDT patch](/x300/acpi-custom-DSDT.patch):

```
# cd /usr/src/linux # patch -p1 \< ../acpi-custom-DSDT.patch
```

**NOTE** : The newest -mm kernels have an option for using a custom dsdt, so you do not need to patch your kernel :)

4) Get the [Intel  
ACPI Source Language Compiler](http://www.intel.com/technology/iapc/acpi/downloads.htm) and compile the DSDT you have downloaded in  
step 1:

```
# gzip -d Dell-Latitude\_X300-AXX-custom.asl.gz # tar zxvfp iasl-linux-xxxxxx.tar.gz # cd iasl-linux # ./iasl -tc ../Dell-Latitude\_X300-AXX-custom.asl
```

**NOTE** : Sometimes a given DSDT is only working for a fixed amount of memory. You might have to fix this in the downloaded DSDT for the RAM in your specific machine. Lookout for a keyword like "SystemMemory" and adjust the value accordingly.

5) Copy the compiled file inside the kernel tree:

```
# cp Dell-Latitude\_X300-AXX-custom.hex /usr/src/linux/drivers/acpi/my-dsdt.hex
```

Your kernel is now ready to compile :)

Once you've booted with the new patched kernel you should be able to use _acpid_ daemon, display the thermal info, battery/power status, use the Fn Keys to adjust brightness, LCD/CRT, etc...)

```
pau@cool[53%]~$ acpi -V Battery 1: discharging, 53%, 01:09:41 remaining Thermal 1: ok, 42.0 degrees C AC Adapter 1: off-line pau@cool[53%]~$
```

#### RELATED PAGES:

[http://acpi.sourceforge.net/dsdt/index.php](http://acpi.sourceforge.net/dsdt/index.php)  
[http://www.cpqlinux.com/acpi-howto.html](http://www.cpqlinux.com/acpi-howto.html)  
[http://www.behnel.de/acpi/samsung-acpi.html](http://www.behnel.de/acpi/samsung-acpi.html)

#### RELATED FILES:

- [/etc/acpi/default.sh](/x300/default.sh)

* * *

### SUSPEND/RESUME

Kernel 2.6:

```
Power Management Options: [\*] Software Suspend (CONFIG\_SOFTWARE\_SUSPEND) [\*] Suspend-to-Disk Support (CONFIG\_PM\_DISK)
```

Pass these parameters to the kernel in your boot loader:

```
resume=/dev/SWAP pmdisk=/dev/SWAP (replace SWAP with your swap partition)
```

If you want to boot without resuming, then use:

```
noresume pmdisk=off
```

- For Swap suspend to Disk using Patrick's implementation (newer code, but untested):

```
# echo disk \> /sys/power/state
```

- For swap suspend to disk using Pavel's implementation (ugly but more reliable):

```
# echo 4 \> /proc/acpi/sleep
```

- For suspend to ram:

```
# echo 3 \> /proc/acpi/sleep
```

#### RELATED PAGES:

file://usr/src/linux/Documentation/power/swsusp.txt  
[http://swsusp.sourceforge.net/compare.html](http://swsusp.sourceforge.net/compare.html)

* * *

### IO APIC

When booting the kernel reports this message:

```
Dell Latitude with broken BIOS detected. Refusing to enable the local APIC.
```

If you want to enable the local APIC you can use this patch: [latitude-apic-enabled.diff](/x300/latitude-apic-enabled.diff) (NOT MUCH TESTED, CAN BE DANGEROUS!!)

#### RELATED PAGES:

[http://www.ussg.iu.edu/hypermail/linux/kernel/0301.2/0820.html](http://www.ussg.iu.edu/hypermail/linux/kernel/0301.2/0820.html)

* * *

### Volume Fn Keys

To make use of volume buttons install hotkeys package:

```
# USE="X gtk xosd" emerge hotkeys
```

To start hotkeys put this command in your `.xinitrc`:

```
hotkeys -t x300 -Z &
```

and get my x300.def file from here.

#### RELATED FILES:

- [~/.hotkeys/x300.def](/x300/x300.def.txt)

* * *

### Synaptics Touchpad

Kernel 2.6:

```
\* Enable PS/2 mouse support (CONFIG\_MOUSE\_PS2) \* Enable synaptics touchpad support (CONFIG\_MOUSE\_PS2\_SYNAPTICS) ???? \* Enable "Event device driver" (CONFIG\_INPUT\_EVDEV)
```

XFree86:

```
# emerge synaptics # zless /usr/share/doc/synaptics-VERSION/README.gz # zless /usr/share/doc/synaptics-VERSION/INSTALL.gz
```

Load the driver by canging the XFree configuration file through adding the line '`Load "synaptics"`' in the module section.

```
Section "InputDevice" &nbsp;&nbsp;&nbsp; Identifier &nbsp;&nbsp;&nbsp; "Touchpad0" &nbsp;&nbsp;&nbsp; Driver &nbsp;&nbsp;&nbsp; "mouse" &nbsp;&nbsp;&nbsp; Option &nbsp;&nbsp;&nbsp; "Protocol" "auto-dev" # auto-dev/psaux/event &nbsp;&nbsp;&nbsp; Option &nbsp;&nbsp;&nbsp; "Device" &nbsp; "Synaptics device" EndSection
```

#### RELATED PAGES:

[http://www.tuxmobil.org/touchpad\_driver.html](http://www.tuxmobil.org/touchpad_driver.html)

#### RELATED FILES:

My [XF86Config](/x300/XF86Config).

* * *

### Intel 855GM Graphics Card

Intel Corp. 82852/855GM Integrated Graphics device (rev 02)

To use GLX and DRI you should select these options in the _device drivers_ section of the kernel 2.6 configuration:

```
Under "Character devices" section: &nbsp;&nbsp;&nbsp; \<\*\> /dev/agpgart (AGP Support) (CONFIG\_AGP) &nbsp;&nbsp;&nbsp; \<\*\> Intel I8xx chipset support (CONFIG\_AGP\_INTEL) &nbsp;&nbsp;&nbsp; [\*] Direct Rendering Manager (CONFIG\_DRM) &nbsp;&nbsp;&nbsp; \<\*\> Intel 830M, 845G, 852GM, 855GM, 865G, 915G (CONFIG\_DRM\_I915)
```

If you want framebuffer support:

```
Under "Graphics support" section: &nbsp;&nbsp;&nbsp; [\*] Support for frame buffer devices (CONFIG\_FB) &nbsp;&nbsp;&nbsp; [\*] VESA VGA graphics support (CONFIG\_FB\_VESA)
```

There isn't BIOS support for sending video to both the built-in LCD screen and external VGA at the same time, so I installed [i810switch](http://vorlon.ces.cwru.edu/~ames/i810switch/) to get both CRT and LCD working at the same time, very useful when doing presentations.

```
# emerge i810switch usage: i810switch [crt on/off] [lcd on/off]
```

#### RELATED PAGES:

[http://www.xfree86.org/~dawes/845driver.html](http://www.xfree86.org/~dawes/845driver.html)  
[http://www.xfree86.org/~dawes/intelfb.html](http://www.xfree86.org/~dawes/intelfb.html)  
[Intel 82852/82855 Graphics Controller Family - AGP GART and DRM kernel modules for Linux](http://downloadfinder.intel.com/scripts-df/filter_results.asp?strOSs=39&strTypes=DRV%2CARC&ProductID=922&OSFullName=Linux*&submit=Go%21)

#### RELATED FILES:

My [XF86Config](/x300/XF86Config).

* * *

### Conexant Windmodem

Intel Corp. 82801DB AC'97 Modem Controller (rev 01)

I have not tested it yet, it may work using drivers from smlink:

```
# emerge slmodem
```

or maybe drivers from linuxant:

```
# emerge hcfpcimodem # hcfpciconfig
```

#### RELATED PAGES:

[ftp://ftp.smlink.com/linux/unsupported/](ftp://ftp.smlink.com/linux/unsupported/)  
[http://www.linuxant.com/drivers/](http://www.linuxant.com/drivers/)

* * *

### IRDA

```
# modprobe irtty\_sir # /etc/init.d/irda start (or irattach /dev/ttyS1 -s)
```

then `cat /proc/net/irda/discovery` should show irda devices found:

```
nickname: SIEMENS S45
```

or irdadump:

```
SIEMENS S45 hint=b124 [PnP Modem Fax IrCOMM IrOBEX]
```

to use it as a modem:

```
# modprobe ircomm-tty # ln -s /dev/ircomm0 /dev/modem
```

* * *

### SoundCard

Load _snd\_intel8x0m_ module, included in 2.6 kernel.

* * *

### Integrated NIC

Load _tg3_ module, included in 2.6 kernel.

* * *

### SD Reader

I have not been able to configure it, this is what `lspci -v` reports:

```
02:03.1 CardBus bridge: Ricoh Co Ltd RL5c476 II (rev ac) Subsystem: Dell Computer Corporation: Unknown device 014f Flags: bus master, medium devsel, latency 168, IRQ 10 Memory at 28002000 (32-bit, non-prefetchable) [size=4K] Bus: primary=02, secondary=07, subordinate=0a, sec-latency=176 Memory window 0: 28c00000-28fff000 (prefetchable) Memory window 1: 29000000-293ff000 I/O window 0: 00004800-000048ff I/O window 1: 00004c00-00004cff 16-bit legacy interface ports at 0001
```

If you know how to get it working, **please let me know**!

* * *

### Gkrellm Plugins

[gkx86info](http://anchois.free.fr/) is a plugin that prints the current clock speed for Gkrellm, you can get my gentoo ebuild [here](/x300/gkx86info-0.0.2.ebuild).

* * *

### Battery tips

#### Tip #1

To blank screen when you don't use your laptop add this to your `~/.xinitrc` file:

```
# set dpms values if we are running on battery STATE=`cat /proc/acpi/ac_adapter/ADP1/state |awk {'print $2'}` if [$STATE == "off-line"]; then xset dpms 120 180 240 else xset -dpms fi
```

#### Tip #2

To display the battery level in your prompt add this to your _.bashrc_ file:

```
export PS1='u@h[`acpi |cut -f 2 -d ","|sed -e "s/ //g"`]w$ '
```

it will produce a promt like this:

```
user@hostname[53%]~$
```

#### Tip #3

Laptop mode is a kernel "mode" that allows you to extend the battery life of your laptop. It does this by intelligently grouping write activity on your disks, so that only reads of uncached data result in a disk spinup. It has been reported to cause a significant improvement in battery life (for usage patterns that allow it). Read more about it [here](http://www.xs4all.nl/~bsamwel/laptop_mode/).

* * *

### Other useful resources

[http://www.guilds.net/machines/600m/](http://www.guilds.net/machines/600m/)  
[http://people.ecsc.co.uk/~matt/linux.html](http://people.ecsc.co.uk/~matt/linux.html)  
[http://www.geocities.com/q20linux/](http://www.geocities.com/q20linux/)  
[http://jrv.oddones.org/x300.html](http://jrv.oddones.org/x300.html)

