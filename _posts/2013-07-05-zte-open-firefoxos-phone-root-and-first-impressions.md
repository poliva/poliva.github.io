---
layout: post
title: ZTE Open FirefoxOS Phone, root and first impressions
date: 2013-07-05 14:32:23.000000000 +02:00
type: post
categories:
- FirefoxOS
tags:
- CVE-2012-4220
- DIAG
- exploit
- firefoxos
- roamer2
- root
- zte
- zte-open
meta:
permalink: "/2013/07/05/zte-open-firefoxos-phone-root-and-first-impressions/"
---
![zte open]({{ site.baseurl }}/assets/images/2013/07/zte-open.jpg)

**ZTE Open** is the first non-developer **FirefoxOS** phone, sold commercially in Spain by Movistar.

It can be rooted using [CVE-2012-4220 aka Qualcomm DIAG root](https://www.codeaurora.org/projects/security-advisories/multiple-issues-diagkgsl-system-call-handling-cve-2012-4220-cve-2012) discovered by Giantpune. This security advisory was released by Qualcomm on November 15, 2012. The ZTE Open has been launched commercially 7 months later and neither ZTE nor Movistar have bothered to patch this security hole, shame on them for selling vulnerable devices to customers.

The ZTE Open comes with kernel 3.0.8 which is also vulnerable to [CVE-2013-2094 (perf\_event) exploit](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2013-2094).

### Root

I took the [exploit by Hiroyuki Ikezoe](https://github.com/hiikezoe/break_setresuid) and adapted it to work on the ZTE Open. The source code is available [here](https://github.com/poliva/root-zte-open), and a _redy-to-use_ compiled exploit is here: [**DOWNLOAD**](/archives/files/root-zte-open.zip).

These are the details of the original firmware, as hopefully ZTE will patch the security hole and this exploit might not work in future versions:

```
ro.build.display.id=OPEN_FFOS_V1.0.0B04_TME
ro.build.sw_internal_version=B2G_P752D04V1.0.0B08_TME
ro.build.firmware_revision=V1.01.00.01.019.120
ro.build.date=Fri May 31 23:10:17 CST 2013
```

To run the exploit connect your phone to your computer using the USB cable, and make sure '_Remote debugging_' is enabled on your phone in Settings -\> Device information -\> More Information -\> Developer.  
You need to have the adb binary in your computer's path, (if you don't know what ADB is don't bother rooting your phone) then execute "run.sh" on Linux or OS X, or "run.bat" on Windows.  
If the exploit fails, reboot your ZTE Open and try again (the linux/MAC version will attempt to do that automatically). Once the exploit is successful it will remount the system partition in read/write mode and copy a setuid "su" binary into _/system/xbin/su_.

### Custom ROMs

The bootloader on the ZTE Open does not allow to flash or boot unsigned code through fastboot protocol. The stock recovery image will verify the signature of update packages and not allow you to flash self-signed updates. To overcome that limitation you can flash a custom recovery image that will allow you to backup your current ROM to SD card and flash your customized build of FirefoxOS (or if you want, your own Android port).

You can download ClockWorkMod recovery for ZTE Open here: [recovery-clockwork-6.0.3.3-roamer2.img](/archives/files/recovery-clockwork-6.0.3.3-roamer2.img).  
To flash it:

```
# first backup your existing recovery
adb shell dd if=/dev/mtd/mtd0 of=/sdcard/stock-recovery.img bs=4k
adb pull /sdcard/stock-recovery.img
	
# then flash clockworkmod recovery
adb push recovery-clockwork-6.0.3.3-roamer2.img /sdcard/cwm.img
adb shell flash_image recovery /sdcard/cwm.img
```

To boot into recovery mode, hold both volume ~~down~~ up and the power button while powering on the phone.

Enjoy! :)

