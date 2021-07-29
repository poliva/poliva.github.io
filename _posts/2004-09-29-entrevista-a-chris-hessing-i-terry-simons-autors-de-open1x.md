---
layout: post
title: Entrevista a Chris Hessing i Terry Simons, autors de Open1x
date: 2004-09-29 13:56:43.000000000 +02:00
type: post
categories:
- linux
- security
- wireless
tags: []
meta:
permalink: "/2004/09/29/entrevista-a-chris-hessing-i-terry-simons-autors-de-open1x/"
---
Matthew Gast, autor del llibre _[802.11 Wireless Networks: The Definitive Guide](http://www.oreilly.com/catalog/802dot11/index.html?CMP=ILL-4GV796923290)_ entrevista a Chris Hessing i Terry Simons, autors de [Open1x](http://www.open1x.org/) --implementació open source de IEEE 802.1x-- en l'artícle titulat _[Wireless Security and the Open1X Project](http://www.macdevcenter.com/pub/a/mac/2004/09/21/open1x.html)_ de O'Reilly Network.

Al artícle parlen de la implementació que han fet per a la universitat de Utah utilitzant TTLS/PAP i WEP dinàmic, de la integració amb l'autenticació kerberos que tenen pensat fer próximament, de la posibilitat d'utilitzar un entorn mixte amb WPA i WEP al mateix temps, també parlen de [EduRoam](http://www.eduroam.nl/en/index.shtml), un projecte per fer roaming entre universitats que s'ha implementat a varies universitats holandeses, de la importància que tindràn en el futur EAP-SIM i EAP-AKA, i dels problemes que tenen alguns drivers per a linux que resetejen la tarjeta cada cop que es configuren les claus WEP, fet que provoca resets constants si s'utilitza WEP dinàmic. En resum, una entrevista molt recomanable, llàstima que no mencionen res sobre el [wpa\_supplicant](http://hostap.epitest.fi/wpa_supplicant/) de Jouni Malinen, que actualment em sembla que està molt més avançat que el seu pròpi xsupplicant.

