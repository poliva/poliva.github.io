---
layout: post
title: Why Broadcom 802.11 Linux STA driver sucks, and how to fix it
date: 2012-05-23 00:49:17.000000000 +02:00
type: post
categories:
- linux
- wireless
tags:
- brcmsmac
- broadcom
- CFG80211
- dkms
- kernel
- lkm
- module
- networkmanager
- ubuntu
- WEXT
- wifi
- wireless
- wl
- wpasupplicant
meta:
permalink: "/2012/05/23/why-broadcom-80211-linux-sta-driver-sucks-and-how-to-fix-it/"
---
<blockquote>TL;DR - the broadcom sta linux driver always fails in the first scan request after the interface is brought up, this produces a long delay when connecting to a wireless network. There's an open source driver which does not have this problem, but is not good with power management. In this post I describe the steps I took to pinpoint the problem in the proprietary driver and to fix it.</blockquote>

The story begins when I updated Ubuntu from 11.10 to 12.04 on my MacBook Air, everything worked fine after upgrading except one thing that bothered me a lot: when resuming the laptop after suspending it, it took around 30 seconds to connect to my wireless network. It wouldn't have bothered me if it had been the same in 11.10, but in 11.10 the time to connect was barely 5 or 6 seconds, so having to wait 30 seconds was totally unacceptable.

Initially I thought it was a bug in NetworkManager, and increased the debug level in the config file to finally come out to the conclusion that I was using a different driver in 12.04 than in 11.10.

There are two drivers available for the Broadcom BCM4353 802.11 Wireless Controller:
<ul>
<li><strong><a title="Broadcom brcmsmac" href="http://linuxwireless.org/en/users/Drivers/brcm80211">Broadcom brcmsmac (mac80211-based softmac PCIe)</a></strong>: the completely open source drivers, included in the kernel</li>
<li><strong><a title="Broadcom 802.11 Linux STA driver" href="http://www.broadcom.com/support/802.11/linux_sta.php">Broadcom 802.11 Linux STA driver</a></strong>: the broadcom mixed GPL source + a proprietary hybrid binary file agnostic to the specific version of the Linux kernel</li>
</ul>
<!--more-->
Both wl (broadcom proprietary driver) and brcmsmac (the open source driver) were installed in my Ubuntu 11.10 but the open source driver was used by default, and this driver connected to the wifi network in 5 seconds.

In Ubuntu 12.04, the wl proprietary driver provided by the package bcmwl-kernel-source has been updated from version 5.100.82.38+bdcom-0ubuntu4 to version 5.100.82.38+bdcom-0ubuntu6.1 which includes the following fix:
```
---------------
bcmwl (5.100.82.38+bdcom-0ubuntu6.1) precise-proposed; urgency=low
	
  * debian/bcmwl-kernel-source.postinst:
    - Blacklist brcmfmac, brcmsmac and bcma so that they don't
      conflict with the closed driver (LP: #873117)
 -- Alberto Milone  Mon, 23 Apr 2012 16:11:56 +0200
```

Which basically blacklists the open source brcmsmac module, forcing the wl proprietary driver to be in use. When the brcmsmac was not blacklisted, even if the wl driver was loaded it failed silently and brcmsmac was used instead.

So, the easy path to solve my problem would have been to blacklist the wl module, and add the brcmsmac to <tt>/etc/modules</tt> and live happy with my 5 seconds needed to associate when resuming, **\*BUT\*** I compared both drivers and the proprietary driver has better signal and way better power management, which makes my battery last longer, so I decided to go the long route. My goal was to achieve the lowest delay possible to connect to a wireless network when coming from a suspend using the proprietary driver.

I went "down" one level and started looking at wpasupplicant, as NetworkManager communicates with it using the DBus control interface (dbus-monitor showed the problem was not in dbus communication) so, increased the debug level in wpa-supplicant by adding '<tt>-dd</tt>' switch in <tt>/usr/share/dbus-1/system-services/fi.w1.wpa_supplicant1.service</tt>, and looked through the logs, which quickly revealed the following:

```
May 20 11:49:46 maco wpa_supplicant[12610]: Scan requested (ret=0) - scan timeout 5 seconds
May 20 11:49:52 maco wpa_supplicant[12610]: Scan timeout - try to get results
May 20 11:49:52 maco wpa_supplicant[12610]: Failed to get scan results
May 20 11:49:52 maco wpa_supplicant[12610]: Failed to get scan results - try scanning again
May 20 11:50:07 maco wpa_supplicant[12610]: Scan requested (ret=0) - scan timeout 5 seconds
```

The first scan request (SIOCSIWSCAN), after the wireless interface was brought up always failed (?), and wpa\_supplicant tried to get the scan results (SIOCGIWSCAN) because some drivers do not deliver SIOCGIWSCAN events to notify when scan is complete, but this failed too, so wpasupplicant requested a second scan after a timeout, which properly delivered the results this time. This first failing scan was adding 21 seconds of delay to the network association process.

I googled the error and found I was not the only soul affected by this problem, Kalle Valo submitted 4 different patches to the hostap mailing list between October 2010 and March 2011, but the patches were never accepted upstream, nor included in the ubuntu package. The wpasupplicant code has changed a bit since Kalle submitted his patches, so I adapted them to the current wpa\_supplicant version in Ubuntu. If you are curious, you can dig through [ubuntu bug #994739](https://bugs.launchpad.net/bugs/994739 "wireless takes several seconds longer to connect from standby").

In short, the [version 4 patch](http://lists.shmoo.com/pipermail/hostap/2011-March/022891.html "Add a workaround for Broadcom wl driver's first failing scan") from Kalle basically patches the WEXT driver from wpasupplicant to check the return value when trying to get scan results (SIOCGIWSCAN) from the wl driver, if the number of last error (errno) is `EINVAL` on the first scan, it requests another scan, so this one will go through (as only the first one fails) and return the scan results next time wpasupplicant tries to get them. This is far from perfect, but it works (and doesn't seem to break anything), reducing the time needed to associate to the wireless network after the interface has been brought up from 30 seconds to 12 seconds.

But I was still unhappy with this result, so I [patched](/archives/files/mba42/wpa-supplicant-fix-wl-driver.patch "wpa supplicant fix wl driver patch") the wpasupplicant code to request a scan right after the driver init function, so this would be the first "failing" scan, and the real scan requested a bit later would return results. This was a very ugly patch, because it made wpasupplicant request a scan in INACTIVE state (when it should be SCANNING), but it worked and reduced the time from 30 seconds to 10 seconds.

So, still unhappy with the results, I decided to go down one more level and have a look at the GPL'd source of the Broadcom's Linux STA proprietary driver, and BINGO! this is how the `wl_iw_set_scan()` function ends:

```
        (void) dev_wlc_ioctl(dev, WLC_SCAN, &ssid, sizeof(ssid));
	
        return 0;
```

They always return 0, even when the `dev_wlc_ioctl()` function fails!! and WTH is it casted to void?? It would have been easier to just return the result of this function!. Patching this shows that the first scan after the interface is up fails with errno `EBUSY` (device or resource busy), so I added a workaround here to make it request the scan to the underlying hardware until it returned something different than `EBUSY` and could be correctly handled by wpasupplicant, _et voilà_, time reduced to 10 seconds.

But hey, now that I looked at their source, it turns out that there's a newer version available in broadcom's website: 5.100.82.112. This version now supports the new linux cfg80211 wireless configuration API in addition to the older Wireless Extensions (WEXT), you can choose between CFG80211 or WEXT at compile time, the ubuntu package <tt>broadcom-sta-dkms</tt> in the development release for 12.10 'Quantal Quetzal' has been updated to this version but still uses the old WEXT which is still broken (always returns 0, remember above?). But, guess what they have done it correctly this time in the new CFG80211 code, see the end of the function `__wl_cfg80211_scan()`:

```
        err = wl_dev_ioctl(ndev, WLC_SCAN, &sr->ssid, sizeof(sr->ssid));
        if (err) {
                if (err == -EBUSY) {
                        WL_INF((\"system busy : scan for \\"%s\\" \"
                                \"canceled\n\", sr->ssid.SSID));
                } else {
                        WL_ERR((\"WLC_SCAN error (%d)\n\", err));
                }
                goto scan_out;
        }
	
        return 0;
	
scan_out:
        clear_bit(WL_STATUS_SCANNING, &wl->status);
        wl->scan_request = NULL;
        return err;
```

As you can see, they now return `EBUSY` when the driver cannot perform the scan, and wpa\_supplicant can manage this situation correctly, so I quickly backported the <tt>broadcom-sta-dkms</tt> package from ubuntu 12.10 to ubuntu 12.04 and added a patch to compile it with CFG80211 enabled, and finally **I CAN HAS ONLY 8 SECONDS DELAY!!!!1** to associate to the wifi network after my laptop resumes from suspend using the wl driver, and I'm a happy camper! :D

