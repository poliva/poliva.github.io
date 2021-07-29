---
layout: post
title: DD-WRT v24 suporta multiples SSIDs
date: 2006-07-06 04:15:27.000000000 +02:00
type: post
categories:
- gadgets
- linux
- wireless
tags: []
meta:
  tags: dd-wrt linksys wrt54g firmware virtual+AP VAP
permalink: "/2006/07/06/dd-wrt-v24-suporta-multiples-ssids/"
---
L'altre dia comentava el tema dels [Virtual Access Points](/blog/2005/02/15/ap-amb-multiples-ssids-virtual-ap/), concretament parlava sobre la [implementació de madwifi-ng](/blog/2006/06/30/traient-li-el-suc-a-madwifi-ng/) per a xips Atheros; donçs ara acabo de flashejar un dels Linksys WRT45G que tinc per casa amb la [versió v24 alpha de DD-WRT](http://www.dd-wrt.com/dd-wrtv2/down.php?path=downloads%2Funtested_alpha_unstable%2Fdd-wrt.v24+very+alpha/) i sorpresa, també té suport de Virtual APs!!

He configurat dos SSIDs: un obert amb [chillispot](http://www.chillispot.org/) fent de portal captiu i un amb WPA-PSK amb bridge a la LAN cablejada, he hagut de toquetejar una mica els scripts d'arranc per a crear correctament la configuració del chilli ja que a través de la interface web només et deixa configurarlo per al LAN+WLAN (br0), LAN o WAN, i jo volía que només estigués al `wl0.1`, es a dir el SSID obert. Suposo que quan surti la v24 final aquests detallets ja es podrán tocar via web.

![dd-wrt v24 VAP]({{ site.baseurl }}/assets/images/2006/07/ddwrt24-2.jpg)

<!--more-->

El cas és que m'he quedat bastant sorprés al veure la _feature_ de multiples SSIDs implementada amb el xipset Broadcom que porten els Linksys, ja que aquesta opció no la suporta Broadcom ni tansols està inclosa en el firmware original de Linksys per als wrt54g, no sé com s'ho haurà fet brainslayer, però aquest cop "em trac el sombrero".

![dd-wrt v24 VAP]({{ site.baseurl }}/assets/images/2006/07/ddwrt24-1.jpg)

Després d'unes quantes hores fent proves amb diferents dispositius i capturant les trames en mode monitor val a dir que la implementació és bastant _cutre_, ja que tots els SSIDs es publiquen amb el mateix BSSID, i això dona molts de problemes en segons quins drivers. Aquí teniu la captura d'una trama on es veu el SSID obert (mac de `wl0.1`) amb el BSSID de la interface física (mac de `wl0`):

![dd-wrt v24 VAP]({{ site.baseurl }}/assets/images/2006/07/ddwrt24-ethereal.png)

- 

Amb un client **Linux** amb xipset **Atheros** (driver madwifi-ng) no he tingut cap problema, veig els dos SSIDs correctament i puc escollir el que vulgui i associar-me perfectament.

- 

Amb un client **WindowsXP SP2** amb xipset **Atheros** quan obtinc la llista de les xarxes sense fils disponibles només veig el SSID <tt>pofHQ-Open</tt>, però de vegades em diu que és una xarxa no segura i de vegades em diu que té WPA habilitat. Es impossible associar-se (a cap dels dos SSIDs) i obtenir connectivitat.

- 

Amb una PDA amb **Windows Mobile 2003** amb xipset **Texas Instruments** només veig el SSID <tt>pofHQ-Open</tt> i és al únic al que em puc associar.

- 

Amb un client **MacOS X** amb xipset **Broadcom** només veig el SSID <tt>pofHQ-Open</tt> i és al únic al que em puc associar.

A part dels múltiples SSIDs (amb un sól BSSID, grr!) el [DD-WRT](http://www.dd-wrt.com/) v24 té moltes més opcions i és un dels firmwares més complerts que he vist fins ara per als Linksys, si el voleu provar heu de tenir en compte que encara és una versió _alpha_ i que pot donar problemes, però té coses molt <acronym title="feu un strings del binary de httpd i busqueu FON :P">interessants</acronym> per trastejar ;)

