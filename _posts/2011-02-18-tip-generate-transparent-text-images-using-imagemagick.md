---
layout: post
title: "[tip] generate transparent text images using imagemagick"
date: 2011-02-18 12:38:07.000000000 +01:00
type: post
categories:
- linux
- minipost
tags:
- image
- imagemagick
- text
- tip
meta:
  yourls_tweeted: '1'
  _wp_old_slug: ''
permalink: "/2011/02/18/tip-generate-transparent-text-images-using-imagemagick/"
---
To generate 100 images, containing numbers 0 to 100:

```
for f in `seq 0 100` ; do convert -size 25x25 xc:transparent -font /usr/share/fonts/truetype/ttf-dejavu/DejaVuSansMono-Bold.ttf \ -fill $COLOR -pointsize 19 -draw "text 1,20 '$f'" res/drawable/notify\_$f.png done
```
