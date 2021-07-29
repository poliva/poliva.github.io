---
layout: post
title: Actualització del servidor per reduir soroll i temperatura
date: 2006-05-12 03:17:36.000000000 +02:00
type: post
categories:
- pofHQ
tags: []
meta:
  tags: pofhq server hardware harddisk cpu temperature noise
permalink: "/2006/05/12/actualitzacio-del-servidor-per-reduir-soroll-i-temperatura/"
---
[![]({{ site.baseurl }}/assets/images/2006/05/s0.jpg)](/photos/folder/albums/FOTOS_SERVER_POFHQ/)

Fa cosa d'un mes que li vaig fer una actualització de hardware al servidor que allotja aquestes pàgines, la meva idea era reduir el soroll i fer baixar la temperatura dels components (CPU, HD, etc...). Actualment el servidor té una CPU [AMD Athlon XP 2000](http://www.amd.com/us-en/Processors/ProductInformation/0,,30_118_3734,00.html) (1666Mhz), amb una placa base [MSI KT3 Ultra-ARU](http://www.msi.com.tw/program/products/mainboard/mbd/pro_mbd_detail.php?UID=11) i 1,5 Gb de RAM en 2 DIMMS [Kingston](http://www.kingston.com/) a 333Mhz. Aquest hardware és el que tinc funcionant des de fa mes o menys 4 anys, només li he ampliat la RAM i els discs durs, que finalment han quedat així:

- 1 x 40 Gb [Seagate](http://www.seagate.com/) UDMA(100) per al sistema
- 2 x 200 Gb [Maxtor](http://www.maxtor.com/) UDMA(133) en raid1 per al <tt>/home</tt>
- 1 x 120 Gb [Maxtor](http://www.maxtor.com/) UDMA(133) per a backup
- 1 x 300 Gb [Maxtor](http://www.maxtor.com/) UDMA(133) per a storage poc important (no té backup ni raid)

La caixa on tenia muntat abans el servidor era bastant _cutrilla_ (de 30eur caixa + font) i tot el conjunt s'acalentava moltissim, per això vaig posar-li uns quants ventiladors i feia un soroll increïble, bastant insuportable tenint en compte que el server i jo dormim a la mateixa habitació.

El nou hardware que he posat per complir l'objectiu de reducció de soroll i temperatura és el següent:

- Caixa [Tsunami VA3000SWA](http://www.thermaltake.com/product/Chassis/midtower/tsunami/swa/swa.asp)
- Font: [Raidmax RX-450 Power Viking](http://www.raidmax.com/Products/performancePSU/rx450/rx450.html)
- Potenciòmetres per als ventiladors [Xcontroller A2140](http://www.thermaltake.com/product/Accessory/DriveBay/a2140/a2140.asp)
- Refrigerador de HD 3.5": [Aplus Case SURF](http://www.maxpoint.de/en/pages/products/case_list.php?we_objectID=2040) de [A+Case](http://www.aplus-case.de/).

Per fer-me una idea de com ha millorat el tema amb aquest nou hardware he fet una taula amb la temperatura mitja dels discs durs, la CPU i el xipset de la placa base, comparant els valors mitjans anteriors i posteriors al canvi. Podeu veure la evolució a les [gràfiques](/graphs/?op=day) que faig amb rrdtool.

| **Component** | **Temp. anterior (ºC)** | **Temp. actual (ºC)** | **Disminució (ºC)** | **Percentatge (%)** |
| **HDA** (sistema) | 55 | 36 | 19 | 35% |
| **HDC** (backup) | 48 | 29 | 19 | 40% |
| **HDD<sup>*</sup>** (storage) | -- | 37 | -- | -- |
| **HDE** (raid1) | 51 | 33 | 18 | 36% |
| **HDG** (raid1) | 56 | 33 | 23 | 41% |
| **CPU** | 57 | 50 | 7 | 13% |
| **SYS** (xipset) | 49 | 39 | 10 | 21% |

**\*** Aquest disc és una nova incorporació, per tant no tinc dades històriques.

Com podeu veure, en alguns casos la temperatura ha baixat fins a un 40%!!. Respecte al soroll la cosa ha millorat bastant, sobre tot perquè puc controlar les RPM dels ventiladors amb els potenciòmetres del frontal i quan vaig a dormir els poso al mínim, tot i així continua fent una mica de soroll però no tant com abans, ara es més suportable :)

Els curiosos, aquí teniu les [fotos del upgrade del server](/photos/folder/albums/FOTOS_SERVER_POFHQ/).

