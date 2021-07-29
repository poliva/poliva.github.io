---
layout: post
title: "[tip] Change colors using imagemagick"
date: 2011-02-18 11:25:30.000000000 +01:00
type: post
categories:
- linux
- minipost
tags:
- color
- image
- imagemagick
- tip
meta:
  yourls_tweeted: '1'
  _wp_old_slug: ''
permalink: "/2011/02/18/tip-change-colors-using-imagemagick/"
---
This will replace all black color for white color:  
`convert input.png -fill white -opaque black output.png`

