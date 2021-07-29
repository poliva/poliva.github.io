---
layout: post
title: WiFite patch for WLAN/JAZZTEL networks WEP & WPA cracking
date: 2011-03-21 00:45:30.000000000 +01:00
type: post
categories:
- linux
- security
- wireless
tags:
- cracking
- jazztel
- patch
- python
- wep
- wifite
- wlan
- wpa
meta:
  yourls_tweeted: '1'
permalink: "/2011/03/21/wifite-patch-wlan-jazztel-wep-wpa-cracking/"
---
I have made a quick patch for [wifite](http://code.google.com/p/wifite/) r67, which adds support to crack WLAN and JAZZTEL networks in Spain, both WEP and WPA versions.

The WPA keys are computed statically using [the already known algorithms](/2011/02/04/algoritmo-para-generar-claves-wpa-de-las-redes-wlan_xxxx-y-jazztel_xxxx/) and the guessed key is shown at start, when wifite shows the available networks.

The WEP keys are cracked using a dictionary attack, generated automatically using [wlandecrypter](http://foro.seguridadwireless.net/aplicaciones-y-diccionarios-linux/wlandecrypter-0-6-nuevas-macs/) and [jazzteldecrypter](http://foro.seguridadwireless.net/aplicaciones-y-diccionarios-linux/jazzteldecrypter/), so you only need 4 IVs to start cracking using the dictionary.

The patch is available [here](http://x90.es/xw), and it's agains revision r67 (latest at the moment of writing this). You can see it in action in these youtube videos I have uploaded:  
<!--more-->  
<iframe title="YouTube video player" width="960" height="750" src="http://www.youtube.com/embed/_eZxcRGB8jc?rel=0&amp;hd=1" frameborder="0" allowfullscreen></iframe>

[Cracking WPA using patched wifite r67](http://www.youtube.com/watch_popup?v=_eZxcRGB8jc&vq=hd720): Shows how to crack 9 WPA networks in less than 30 seconds.

<iframe title="YouTube video player" width="960" height="750" src="http://www.youtube.com/embed/Rxh9XYFHvDI?rel=0&amp;hd=1" frameborder="0" allowfullscreen></iframe>

[Cracking WEP using patched wifite r67](http://www.youtube.com/watch_popup?v=Rxh9XYFHvDI&vq=hd720): Shows how to crack a WEP network with clients in 11 seconds, and a WEP network without any clients connected in 1 min 11 seconds.

