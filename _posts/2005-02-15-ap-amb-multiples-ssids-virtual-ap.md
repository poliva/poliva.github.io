---
layout: post
title: AP amb múltiples SSID's (Virtual AP)
date: 2005-02-15 16:05:22.000000000 +01:00
type: post
categories:
- linux
- wireless
tags: []
meta:
permalink: "/2005/02/15/ap-amb-multiples-ssids-virtual-ap/"
---
<p>Els access points amb múltiples SSID ofereixen avantatges sobre els que no tenen aquesta opció, ja que permeten minimitzar els conflictes de canals en llocs com aeroports on conviuen múltiples xarxes separades amb la conseqüent reducció de costos al construir una única xarxa física i compartir-la en lloc de construir-ne varies.</p>
<p>El suport de múltiples SSID depèn del xipset wireless que utilitzi el hardware del AP (concretament de com el firmware gestiona els paquets <em>probe-request</em> i <em>probe-response</em>) i del driver utilitzat per a controlar el xipset.</p>
<p>Gran part dels xipsets existents actualment gestionen els paquets <em>probe-request/response</em> de manera que fa impossible que es puguin tenir múltiples SSID's en una sola radio, inclòs el famós xip Prism2 i és per això que el complert driver <a href="http://hostap.epitest.fi">hostap</a> per a Linux no permet crear múltiples SSID's per software. Evidentment si el xipset ho permetés, aquest driver seria el primer en implementar-ho, però per desgracia no ho permet. Els únics xipsets que ho permeten (fins on jo tinc constància, que algú em corregeixi si m'equivoco) són els Airo (Cisco) i els Atheros, ja que implementen de forma diferent el <em>frame control</em>.</p>
<p>Amb els access points de la serie Aironet de Cisco, es possible definir múltiples SSID's a la interface wireless i associar-los a VLANs diferents a la interface ethernet del AP, de manera que es puguin enviar a un port 802.1q trunk d'un switch Cisco. Cisco de moment només fa broadcast del primer SSID, i per als altres accepta els <em>prove requests</em>, es a dir, que existeixen però no es veuen si fem un escaneig amb programes com kismet o netstumbler. Segons tinc entès, això ho volen solucionar en properes versions de IOS.</p>
<p>Desconec com ho implementa Cisco exactament, però amb els xips Atheros es pot fer ACK de paquets amb adreces MAC diferents de la MAC física de la interface wireless del AP ---cosa que fa possible emetre múltiples SSID's---
però aquesta part del hardware la control·la el <acronym title="Hardware Abstraction Layer">HAL</acronym> del driver [madwifi](http://madwifi.sf.net) que és justament la part de codi tancat i aquesta característica no està implementada.

Existeix hardware amb linux _embedded_ que porta xipset Atheros i sí que té suport de múltiples SSID's, però el driver que inclou és una versió tancada del madwifi, que Atheros només distribueix als fabricants que firmen un <acronym title="Non Disclosure Agreement">NDA</acronym> com Colubris, Airmagnet o 3Com entre d'altres.

Per a ampliar els coneixements sobre maneres d'implementar múltiples SSID's recomano la lectura de [Virtual Access Points (ppt)](http://www.drizzle.com/~aboba/IEEE/virtual-APs.ppt) de Bernard Aboba. Per a més informació deixo una llista de llocs on s'ha tractat aquest tema anteriorment: [1](http://lists.shmoo.com/pipermail/hostap/2003-October/004570.html), [2](http://lists.shmoo.com/pipermail/hostap/2003-September/004146.html), [3](https://amsterdam.lcs.mit.edu/pipermail/click/2004-October/003284.html) i [4](http://openwrt.org/logs/wrt54g.log.20041120).

