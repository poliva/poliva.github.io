---
layout: post
title: Building debian packages from source
date: 2008-06-05 01:51:50.000000000 +02:00
type: post
categories:
- linux
- minipost
tags: []
meta:
  tags: debian ubuntu package deb building source
permalink: "/2008/06/05/building-debian-packages-from-source/"
---
I will show how to build a debian package from source, needed if you need to apply a custom patch to some package.

1. Download the debian source

```
# apt-get source package-name
```

2. Download the dependencies required to build the source

```
# apt-get build-dep package-nam
```

3. Patch the source as needed.

4. Build the debian package:

```
# cd source-dir # debuild -us -uc
```

The recompiled .deb file will be in the parent directory, you can install it with dpkg as usual.

