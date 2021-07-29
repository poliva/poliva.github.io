---
layout: post
title: 'lightum: auto adjust keyboard brightness on Linux MacBook'
date: 2011-12-04 11:52:44.000000000 +01:00
type: post
categories:
- linux
tags:
- github
- lightum
- macbook
- ppa
- ubuntu
meta:
  yourls_tweeted: '1'
permalink: "/2011/12/04/lightum-auto-adjust-keyboard-brightness-on-macbook/"
---
If you are running Linux on your MacBookAir and want the keyboard light brightness changed automatically depending on the ambient light (just as OS X does), continue reading.

I've written a small daemon named [lightum](https://github.com/poliva/lightum), source is on github and licensed under GPL-2+.

```
Usage: lightum [-m value] [-p value] [-f] -m 0..255 : maximum brightness value between 1 and 255 (default=255) -p num : number of seconds between light sensor polls (default=8) -f : run in foreground (do not daemonize) -v : verbose mode, useful for debugging with -f
```

For it to work you need dbus installed, and your MacBook should have the light sensor located in <tt>/sys/devices/platform/applesmc.768/light</tt> (should be available on all MacBookAir and MacBookPro versions which have a backlight on keyboard, as far as I know).

If you are running Ubuntu, you can install it by adding [lightum-mba ppa](https://launchpad.net/~poliva/+archive/lightum-mba) to your system:  
`
sudo add-apt-repository ppa:poliva/lightum-mba
sudo apt-get update
sudo apt-get install lightum
`

Otherwise, you can build it from [source](https://github.com/poliva/lightum/tarball/master).

