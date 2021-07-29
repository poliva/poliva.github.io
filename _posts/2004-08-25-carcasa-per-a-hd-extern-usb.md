---
layout: post
title: Carcasa per a HD extern USB
date: 2004-08-25 23:12:20.000000000 +02:00
type: post
categories:
- gadgets
- personal
tags: []
meta:
  tags: ''
permalink: "/2004/08/25/carcasa-per-a-hd-extern-usb/"
---
Ahir em vaig comprar una carcasa [ConStar ST-2312B2 USB 2.0](http://www.snt.com.tw/product.php?mode=show&cid=21&pid=22) i un disc dur [Maxtor](http://www.maxtor.com) de 200Gb (7200 RPM i 8Mb buffer). La veritat es que estic bastant content amb l'invent, que em permet transportar una bona quantitat d'informació i amb un preu raonable (41 euros la carcasa i 89 euros el disc). La capsa és tota d'alumini i dissipa bastanta calor, sobre tot quan el disc porta una bona estona funcionant, al hivern podrà servir d'estufa :)

El disc IDE treballant a través de USB 2.0 (480Mb/s) dóna una tasa de transferencia molt acceptable:

```
# hdparm -tT /dev/sda /dev/sda: Timing buffer-cache reads: 1144 MB in 2.00 seconds = 571.23 MB/sec Timing buffered disk reads: 60 MB in 3.05 seconds = 19.64 MB/sec
```

Li vaig fer 2 particions, una ext3 de 6Gb al principi del disc (per poder instalar-hi un linux si algun dia em fa falta arrancar per USB) i la resta de disc (194Gb) en una partició FAT32, si has llegit bé: FAT32. La partició FAT32 la vaig haver de fer desde Linux, ja que WinXP no et permet fer particions FAT32 mes grans de 32 Gb (t'obliga a utilitzar NTFS si la vols més gran), però si que les reconeix si les formateges amb un altre S.O., em sembla que el límit per a una partició FAT32 es de 2 TeraBytes. D'aquesta manera puc utilitzar-lo per llegir i escriure dades desde els dos sistemes operatius.

