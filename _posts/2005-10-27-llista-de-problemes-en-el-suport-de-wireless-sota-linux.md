---
layout: post
title: Llista de problemes en el suport de wireless sota Linux
date: 2005-10-27 01:37:43.000000000 +02:00
type: post
categories:
- linux
- wireless
tags: []
meta:
  tags: linux wireless dbus kernel device driver network api
permalink: "/2005/10/27/llista-de-problemes-en-el-suport-de-wireless-sota-linux/"
---
![NetworkManager Architecture]({{ site.baseurl }}/assets/images/2005/10/architecture.png)

Dan Williams ha enviat [aquest missatge](http://www.ussg.iu.edu/hypermail/linux/kernel/0501.3/0667.html) a la llista de correu del kernel on reporta tots els petits problemes, inconsistències i diferències entre els drivers per a dispositius wireless (802.11x) existents actualment per a Linux. El llistat de problemes ha sorgit gràcies a l'esforç que està fent per facilitar la gestió de dispositius de xarxa en mode GUI amb [NetworkManager](http://people.redhat.com/dcbw/NetworkManager/), un dimoni que interactua directament amb el hardware i drivers per gestionar els dispositius de xarxa i permet la comunicació amb altres programes mitjançant [dbus](http://www.freedesktop.org/Software/dbus).

En aquest enllaç podeu veure la [llista de problemes resumida](http://people.redhat.com/dcbw/NetworkManager/linux-wireless-problems.txt), espero que tot això s'unifique i aviat i puguem gaudir d'un suport de dispositius wireless com cal.

