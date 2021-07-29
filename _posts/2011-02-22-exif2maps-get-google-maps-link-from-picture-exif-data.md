---
layout: post
title: exif2maps - get google maps link from picture exif data
date: 2011-02-22 11:33:08.000000000 +01:00
type: post
categories:
- linux
- minipost
tags:
- exif
- geolocation
- google
- location
- maps
- photo
- picture
meta:
  yourls_tweeted: '1'
  _oembed_5d94b4ebe698cbe656b075f5c3b9aa92: "{{unknown}}"
  _wp_old_slug: ''
  _oembed_aa7a4f58a6c38371a6c6d5fbd83a28f7: "{{unknown}}"
  _oembed_ed1f0edda608f0944d80b6a28eb5ae8c: "{{unknown}}"
  _oembed_17dc039c1234524500c24a7c0bfc7483: "{{unknown}}"
permalink: "/2011/02/22/exif2maps-get-google-maps-link-from-picture-exif-data/"
---
A quick shell script to generate a [google maps](http://maps.google.com) link from geo-location data found inside a picture exif tags.  
Usage example:

```
$ ./exif2maps.sh la-foto-3.jpg http://maps.google.com/maps?q=40.9673333,-5.6058333
```

Download: **[exif2maps.sh](/archives/files/exif2maps.sh)**

