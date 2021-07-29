---
layout: post
title: 'Hint: take screenshot on Google Nexus One'
date: 2010-11-10 10:55:45.000000000 +01:00
type: post
categories:
- HTC
- linux
tags:
- n1
- nexsus one
- screenshot
meta:
  _wp_old_slug: ''
permalink: "/2010/11/10/hint-take-screenshot-on-google-nexus-one/"
---
As simple as this (needs a rooted phone):

```
$ adb pull /dev/graphics/fb0 screenshot.raw $ dd bs=1920 count=800 if=screenshot.raw of=image.rgb32 $ ffmpeg -vframes 1 -vcodec rawvideo -f rawvideo -pix\_fmt rgb32 -s 480x800 -i image.rgb32 -f image2 -vcodec png screenshot.png
```
