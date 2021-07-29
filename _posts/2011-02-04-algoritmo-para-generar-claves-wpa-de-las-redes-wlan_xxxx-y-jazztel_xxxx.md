---
layout: post
title: Algoritmo para generar claves WPA de las redes WLAN_XXXX y JAZZTEL_XXXX
date: 2011-02-04 09:56:55.000000000 +01:00
type: post
categories:
- linux
- wireless
tags:
- crack
- jazztel
- jazztel_xxx
- telefonica
- wifi
- wlan
- wlan_xxxx
- wpa
meta:
  yourls_tweeted: '1'
  _wp_old_slug: ''
permalink: "/2011/02/04/algoritmo-para-generar-claves-wpa-de-las-redes-wlan_xxxx-y-jazztel_xxxx/"
---
El 15 de Diciembre de 2010 la página de SeguridadWireless publico un servicio que calculaba las claves WPA de los SSID WLAN\_XXXX y JAZZTEL\_XXX [screenshot aqui](http://twitpic.com/3g5rrt). No publicaron el código, y al cabo de pocas horas la página dejó de funcionar. Hoy me entero via Twitter de que finalmente [a.s.r ha publicado en forocoches el algoritmo](http://www.forocoches.com/foro/showthread.php?t=2051928) para sacar las claves de estas redes de Telefonica y Jazztel. Neikokz ha puesto un [generador en esta página](http://kz.ath.cx/wlan/), basado en el [script original](http://kz.ath.cx/wlan/bash.txt) de a.s.r.

Aquí teneis una versión modificada del script de a.s.r, que escanea las redes disponibles en busca de los SSIDs afectados, e imprime las claves de las que tengamos disponibles a nuestro alrededor:

```
$ ./calcwlan-ng.sh wlan0 [] CalcWLAN-ng (original source by a.s.r, improved by pof) Scanning wifi networks on interface wlan0 SSID: WLAN\_D679 KEY: a36206418a9691112e29
```

**Update:** Añadido soporte para las redes WLAN\_XXXX con BSSID 00:1F:A4 de los routers ZyXEL P660HW-B1A, basado en el algoritmo de [neikokz](http://kz.ath.cx/wlan2/).

**Download: [calcwlan-ng.sh](/archives/files/calcwlan-ng.sh)**

Don't be evil! ;)

