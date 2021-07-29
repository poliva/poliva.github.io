---
layout: post
title: Extract contents of .deb files
date: 2008-06-09 14:04:41.000000000 +02:00
type: post
categories:
- linux
- minipost
tags: []
meta:
  tags: debian ubuntu package deb extract ar
permalink: "/2008/06/09/extract-contents-of-deb-files/"
---
DEB files are _ar_ archives, which contain three files:

- debian-binary
- control.tar.gz
- data.tar.bz2

First we need to extract the _ar_ archive:

```
$ ar vx package.deb
```

Then extract the contents of data.tar.bz2:

```
$ tar xvfp data.tar.bz2
```
