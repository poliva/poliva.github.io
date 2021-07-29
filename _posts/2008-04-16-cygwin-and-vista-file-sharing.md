---
layout: post
title: Cygwin and Vista file sharing
date: 2008-04-16 00:41:21.000000000 +02:00
type: post
categories:
- minipost
tags: []
meta:
  tags: cygwin vista windows permission file sharing
permalink: "/2008/04/16/cygwin-and-vista-file-sharing/"
---
TIP: Cygwin creates shared files in Windows Vista by default, to avoid it add the following line to your `~/.bashrc`:

```
export CYGWIN=nontsec
```
