---
layout: post
title: Connectat amb tecnologia (pre-)WiMAX
date: 2005-11-01 01:35:32.000000000 +01:00
type: post
categories:
- wireless
tags: []
meta:
  tags: ripwave wimax wireless ireland broadband
permalink: "/2005/11/01/connectat-amb-tecnologia-pre-wimax/"
---
![Ripwave CPE]({{ site.baseurl }}/assets/images/2005/11/ripwave.png)  
Aquests dies per Irlanda estic connectat amb la [modalitat Ripwave](http://www.irishbroadband.ie/products_display.php?id=82) de [Irish Broadband](http://www.irishbroadband.ie/) que ténen contractat el [Sergi i l'Eva](/~sergi/). Ofereixen 512Kbps de baixada i 128Kbps de pujada amb un _Contention Ratio_ de 40:1, que vol dir que aquest ample de banda pot arribar a estar compartit entre 40 usuaris simultànis, tot per 18,95/mes (IVA inclòs), sense quotes d'alta ni instal·lació de cap tipus.

[El <acronym title="Customer Premise Equipment">CPE</acronym>](http://www.navini.com/pages/products/cpe.htm) (el router) és un aparell bastant petit fabricat per [Navini Networks](http://www.navini.com/) que funciona a 3,4GHz, actualment amb tecnología pre-WiMAX, però segons asegura el fabricant actualitzable per software a 802.16e (mobile WiMAX) quan es ratifique l'estàndard. S'instal·la a casa, recomanen posar-lo el més elevat possible i prop d'una finestra, i un cop el router té cobertura obtens una IP pública dinàmica a través de DHCP, la gràcia és que **no cal que tingui visió directa** amb l'antena que li dona la cobertura, a diferència de serveis similars que tenim a Espanya com [Iberbanda](http://www.iberbanda.es/) (equipament [Alvarion](http://www.alvarion.com/)), que també funciona amb una xarxa de tipus <acronym title="Wireless Local Loop">WLL</acronym> com la de [Irish Broadband](http://www.irishbroadband.ie/).

![Ripwave PCMCIA]({{ site.baseurl }}/assets/images/2005/11/ripwave_pcmcia.png)  
També existeixen targetes PCMCIA que permeten "emportar-se" la connexió a qualsevol lloc on hi hagi cobertura, i fins i tot el mateix router porta un forat per posar-li una bateria, ideal per muntar un hotspot sobre rodes al cotxe :)

![Ripwave Base Station]({{ site.baseurl }}/assets/images/2005/11/ripwave_bts.png)  
A la web de [Navini](http://www.navini.com/) podeu veure com són les [estacions base](http://www.navini.com/pages/products/bts.htm) per muntar la xarxa troncal, i Irish Broadband té un [mapa de cobertura de Drogheda](http://www.irishbroadband.ie/ws_cov_images/Drogheda.JPG), el poble on viu el [Sergi](/~sergi/).

Ara només penso en com evolucionaran aquests tipus de xarxes: la banda (3,4GHz) no és lliure i s'haurà de fer una bona gestió de l'espectre per a que hi puguin conviure diferents operadors, o fins i tot posar-se d'acord i desplegar una infrastructura d'antenes comú per a explotar-la en conjunt i no fer-se interferència. Espero que els _contention ratios_ no siguin tan exagerats com el que té el [Sergi](/~sergi/) actualment, i que les velocitats augmentin (avui en dia 512Kbps no son rés). També sería interessant veure un router amb _Linux Inside_, on poder-hi muntar el nostre [Asterisk](http://www.asterisk.org/) per utilitzar els serveis d'un <acronym title="Internet Telephony Service Provider">ITSP</acronym> i fer convergir en un sol aparell totalment sense fils tota mena de serveis de telecomunicacions: Internet, Telefonía, Televisió... tot a través d'Internet.

