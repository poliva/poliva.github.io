---
layout: post
title: Guardar la sortida de emerge
date: 2004-09-08 10:20:31.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: ''
permalink: "/2004/09/08/guardar-la-sortida-de-emerge/"
---
Gracies a la [GWN](http://www.gentoo.org/news/en/gwn/20040906-newsletter.xml) d'aquesta setmana he descobert [en aquest thread](http://thread.gmane.org/gmane.linux.gentoo.user/95630) com fer per a poder llegir els missatges que mostren els ebuilds per pantalla quan fem un _emerge_ de varios paquets, el truco consisteix en crear el directori `/var/log/portage` i afegir aquesta línea al `make.conf`:

```
PORT\_LOGDIR=/var/log/portage
```
