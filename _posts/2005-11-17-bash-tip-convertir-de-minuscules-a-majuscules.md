---
layout: post
title: 'Bash tip: convertir de minúscules a majúscules'
date: 2005-11-17 01:09:53.000000000 +01:00
type: post
categories:
- linux
tags: []
meta:
  tags: lowercase uppercase bash tr linux
permalink: "/2005/11/17/bash-tip-convertir-de-minuscules-a-majuscules/"
---
Un apunt curt, només per recordar com es fa això amb la <tt>bash</tt> ja que cada cop que ho haig de fer perdo 5 minuts mirant el <tt>man</tt> del `tr` i per al proper cop segueixo sense recordar-me'n. :evil:

```
cat file.txt **| tr [:lower:] [:upper:]**
```
