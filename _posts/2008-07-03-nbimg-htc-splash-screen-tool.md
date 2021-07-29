---
layout: post
title: 'nbimg: HTC splash screen tool'
date: 2008-07-03 09:39:03.000000000 +02:00
type: post
categories:
- android
- HTC
- linux
tags:
- android
- bmp
- boot
- convert
- htc
- img
- nb
- rgb565
- screen
- splash
meta:
  tags: nbimg nb splashscreen htc
permalink: "/2008/07/03/nbimg-htc-splash-screen-tool/"
---
nbimg is a command line tool which allows to convert HTC Splash Screen images from NB to BMP and create NB splash screens from BMP format. Any splash screen size is supported (yes, it works for Diamond or Athena at 640x480 resolution too).

```
=== nbimg v1.1 === Convert NB \<--\> BMP splash screens === (c)2008 Pau Oliva - pof @ xda-developers Usage: nbimg -F file.[nb|bmp] Mandatory arguments: -F \<filename\> Filename to convert. If the extension is BMP it will be converted to NB. If the extension is NB it will be converted to BMP. Optional arguments: -w \<width\> Image width in pixels. If not specified will be autodetected. -h \<height\> Image height in pixels. If not specified will be autodetected. -t \<pattern\> Manually specify the padding pattern (usually 0 or 255). -p \<size\> Manually specify the padding size. -n Do not add HTC splash signature to NB file. -s Output smartphone format. NBH arguments: (only when converting from BMP to NBH) -D \<model\_id\> Generate NBH with specified Model ID (mandatory) -S \<chunksize\> NBH SignMaxChunkSize (64 or 1024) -T \<type\> NBH header type, this is typically 0x600 or 0x601
```

Example to convert a NB to BMP:

```
$ ./nbimg.exe -F diamond137.nb === nbimg v1.1 === Convert NB \<--\> BMP splash screens === (c)2008 Pau Oliva - pof @ xda-developers [] File: diamond137.nb [] Image dimensions: 480x640 [] Encoding: diamond137.nb.bmp [] Done!
```

Example to convert a BMP to NB:

```
$ ./nbimg.exe -F diamond137.bmp === nbimg v1.1 === Convert NB \<--\> BMP splash screens === (c)2008 Pau Oliva - pof @ xda-developers [] File: diamond137.bmp [] Encoding: diamond137.bmp.nb [] Image dimensions: 480x640 [] Done!
```

### Download

version 1.2+

- Ssource code: [nbimg github page](https://github.com/poliva/nbimg/tags)
- Windows / MacOS X / Linux versions: [nbimg downloads](https://github.com/poliva/nbimg/downloads)

version 1.1

- Linux version (source code): [nbimg-1.1.tar.gz](/HTC/nbimg/nbimg-1.1.tar.gz)
- Windows version: [nbimg-1.1win32.zip](/HTC/nbimg/nbimg-1.1win32.zip)

version 1.0

- Linux version (source code): [nbimg-1.0.tar.gz](/HTC/nbimg/nbimg-1.0.tar.gz)
- Windows version: [nbimg-1.0win32.zip](/HTC/nbimg/nbimg-1.0win32.zip)
