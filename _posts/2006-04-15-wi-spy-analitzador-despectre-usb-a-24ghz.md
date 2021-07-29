---
layout: post
title: 'Wi-SPY: Analitzador d''espectre USB a 2.4GHz'
date: 2006-04-15 23:22:42.000000000 +02:00
type: post
categories:
- gadgets
- linux
- wireless
tags: []
meta:
  tags: wispy usb spectrum+analyzer linux wifi
permalink: "/2006/04/15/wi-spy-analitzador-despectre-usb-a-24ghz/"
---
![]({{ site.baseurl }}/assets/images/2006/04/wispy-dongle.png)

Aquests dies he estat provant un Wi-Spy que hem comprat a la feina, és un _analitzador d'espectre_ que funciona per USB i només escaneja la banda de frequències de 2400 fins a 2483 MHz.

Wi-Spy és útil per detectar la interferècia provocada per dispositius que treballen a la banda lliure de 2,4Ghz, com per exemple xarxes wifi, telèfons inalàmbrics, bluetooth, ZigBee, càmares de vigilància, i fins i tot forns microones que omplenen la banda de soroll si no estàn perfectament apantallats (i els dos als que he apropat el wi-spy no ho estàn!).

El software que porta és molt simple d'utilitzar i permet fer-nos una idea de com està de saturada la banda de frequències que volem utilitzar per escollir el millor canal per colocar un punt d'accés. A continuació podeu veure un parell de captures del software, que està disponible tant per a Linux com per a Windows:

<!--more-->

![]({{ site.baseurl }}/assets/images/2006/04/wispy-linux01.jpg)  
 ![]({{ site.baseurl }}/assets/images/2006/04/wispy-windows01.jpg)

Per instalar el [software de wispy per a Linux](http://www.kismetwireless.net/wispy.shtml) amb Gentoo, només cal fer un <tt>emerge wispy-tools</tt> i si tenim suport de USB al kernel només cal posar-lo al USB i veurem com el sistema el detecta:

```
usb 1-2: new low speed USB device using uhci\_hcd and address 2 usb 1-2: configuration #1 chosen from 1 choice hiddev96: USB HID v1.11 Device [MetaGeek Wi-Spy] on usb-0000:00:1d.0-2
```

Més informació a la web oficial de wispy: [MetaGeek.Net](http://www.metageek.net/).

