---
layout: post
title: Change workspace configuration on Ubuntu 11.10
date: 2011-10-03 20:54:10.000000000 +02:00
type: post
categories:
- linux
- minipost
tags:
- desktop
- gconf
- gnome3
- ubuntu
- unity
- workspace
meta:
  yourls_tweeted: '1'
permalink: "/2011/10/03/change-workspace-configuration-on-ubuntu-11-10/"
---
By default Ubuntu 11.10 with unity interface has a 2x2 workspace, however I am used to have a single row horizontal 4x1 workspace with 4 desktops. I haven't found any option under settings to change this, but here's a workaround:

Edit the file `~/.gconf/apps/compiz-1/general/screen0/options/%gconf.xml` and add two new gconf entries as follows:

```
\<?xml version="1.0"?\> \<gconf\> \<entry name="hsize" mtime="1317329603" type="int" value="4"/\> \<entry name="vsize" mtime="1317329604" type="int" value="1"/\> \</gconf\>
```

Edit the hsize and vsize values to fit your needs :)

