---
layout: post
title: Connexió a 1Mbps
date: 2004-09-24 03:47:11.000000000 +02:00
type: post
categories:
- personal
- pofHQ
tags: []
meta:
  tags: ''
permalink: "/2004/09/24/connexi-a-1mbps/"
---
Fà uns dies vam rebre una carta de auna on ens informaven que podíem doblar la nostra velocitat de connexió a Internet pel mateix preu (47? + IVA) durant 2 mesos, aixi que vam trucar per sol·licitar la oferta i al cap de 2 dies vam passar de una connexió de 300/150 Kbps a una conexió de 600/300 Kbps. El día 22 a la zona de Catalunya [Auna ha duplicat la velocitat de conexió a TOTS els seus clients](http://www.bandaancha.st/weblogart.php?artid=2791) i acomulable a altres ofertes, així que ara tenim una connexió de 1024/300 Kbps pel mateix preu. Això ens durarà fins al 14/11/04 que passaran els dos mesos i sen's acava la oferta inicial, llavors ens deixaràn la linea a 600/300Kbps.

Degut a aquest augment a la velocitat de la connexió he tingut que adaptar el script que utilitzo per gestionar l'ample de banda i quan ja el tenía mes o menys ajustat he fet una prova de _stress testing_ a la connexió, saturant al máxim tant el canal de uplink com el de downlink, aquí teniu els resultats, la zona blava correspón al tràfic interactiu i amb més prioritats (ssh, ack, ping...), la zona verda correspón al tràfic limitat (http, rsync...) i la zona vermella correspón a la resta de tràfic no marcat.

**Downlink**

![downlink]({{ site.baseurl }}/assets/images/2004/09/inetout_hour_eth0.png)

Com es veu a la gràfica, el pic màxim de download és de 143,47 KB/s, amb una mitja de 119,58 KB/s. El límit teóric de 1024 Kbps és 128 KB/s.

**Uplink**

![uplink]({{ site.baseurl }}/assets/images/2004/09/inetout_hour_eth1.png)

En el uplink tenim un màxim de 43,91 KB/s i una mitja de 33,37KB/s, el límit teòric per a 300 Kbps són 37,5 KB/s.

Pròximament publicaré un post amb una petita introducció a [rrdtool](http://people.ee.ethz.ch/~oetiker/webtools/rrdtool/) -- el programa que utilitzo per fer les gràfiques -- i també publicaré els scripts que utilitzo per gestionar l'amplada de banda, per que ja hi ha uns quants que m'ho heu demanat ;)

