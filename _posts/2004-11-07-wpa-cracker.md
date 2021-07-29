---
layout: post
title: WPA cracker
date: 2004-11-07 06:50:39.000000000 +01:00
type: post
categories:
- security
- wireless
tags: []
meta:
permalink: "/2004/11/07/wpa-cracker/"
---
Avui m'he enterat a través de [Wifi Networking News](http://wifinetnews.com/archives/004428.html) de la existència de [WPA Cracker](http://www.tinypeap.com/page8.html), que és una utilitat que realitza atacs de diccionari y força bruta contra <acronym title="Wifi Protected Access">WPA</acronym>.

A diferència dels atacs existents contra el protocol <acronym title="Wired Equivalent Privacy">WEP</acronym>, amb aquest atac que es realitza contra WPA-PSK no cal esnifar una gran quantitat de tràfic, només cal esnifar de forma passiva dos dels paquets <acronym title="EAP over LAN">EAPoL</acronym> que s'intercanvien el client i el punt d'accés durant el procés d'autenticació, concretament els primers dos paquets del _4way-handshake_ que esdevé en la autenticació <acronym title="WPA Preshared Key">WPA-PSK</acronym>.

Un cop s'han esnifat aquests dos paquets (amb [ethereal](http://www.ethereal.com/) per exemple) només cal executar `wpa_crack` i esperar a trovar la passphrase amb el diccionari proporcionat, o provar amb força bruta. Actualment wpa\_cracker es capaç de comprovar 16 passphrases per segon amb atac de diccionari o 18 passphrases per segón si utilitzem força bruta, amb un Pentium M a 1400Mhz.

Més detalls sobre la vulnerabilitat que aprofita:  
· [WPA Passive Dictionary Attack Overview](http://www.tinypeap.com/docs/WPA_Passive_Dictionary_Attack_Overview.pdf) de TakehiroTakahashi.  
· [Weakness in Passphrase Choice in WPA Interface](http://wifinetnews.com/archives/002452.html) de Robert Moskowitz.

