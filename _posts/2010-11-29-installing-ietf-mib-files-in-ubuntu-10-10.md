---
layout: post
title: Installing IETF MIB files in ubuntu 10.10
date: 2010-11-29 11:07:33.000000000 +01:00
type: post
categories:
- linux
- minipost
tags:
- MIB
- snmp
- ubuntu
meta:
  _wp_old_slug: ''
permalink: "/2010/11/29/installing-ietf-mib-files-in-ubuntu-10-10/"
---
Ubuntu 10.10 (and I assume other Debian based distributions too) come without the IETF MIB files installed with the default Net-SNMP package.  
This means you can't query OIDs directly by name anymore, such as "system.sysUpTime.0".

The easiest way to install the missing MIB files, is using the _snmp-mibs-downloader_ package:

```
$ sudo apt-get install snmp-mibs-downloader $ sudo download-mibs $ sudo sed -i 's/^mibs/#mibs/g' /etc/snmp/snmp.conf
```
