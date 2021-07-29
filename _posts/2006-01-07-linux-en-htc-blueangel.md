---
layout: post
title: Linux en HTC BlueAngel
date: 2006-01-07 17:30:41.000000000 +01:00
type: post
categories:
- gadgets
- HTC
- linux
tags: []
meta:
  tags: linux pda2k blueangel htc xanadux familiar openembedded GPE OPIE handhelds
permalink: "/2006/01/07/linux-en-htc-blueangel/"
---
![xanadux]({{ site.baseurl }}/assets/images/2006/01/xanadux.png)

Aquests tres últims dies he estat liat posant-li linux a la meva PDA2k (per fi!). El procés és bastant senzill un cop saps que s'ha de fer i com s'ha de fer, així que aquí queda documentat per si algú més ho vol provar.

Linux arranca desde una tarjeta SD, per tant no hem de flashejar la PDA ni rés per l'estil, no perdem el Windows Mobile que tinguem a la ROM, però si que perdrem totes les dades que hi tinguem, ja que per "apagar" el Linux i tornar a windows no hi ha cap altra manera que fer un _hard reset_, així que abans de fer res, feu un backup :)

Per preparar la SD necessitarem un lector de tarjetes que funcioni amb linux, jo m'he comprat un 32-in-1 Memory Card Reader intern CR202A (marca no t'hi fixis) per 7,90€ que llegeix tot tipus de targetes i funciona perfecte amb linux.

La SD haurà de ser com a mínim de 64Mb, si és més gran millor ja que hi podrém instal·lar més aplicacions. El primer pas es crear 2 particions a la SD. A la primera partició amb format FAT posarem el <tt>haret.exe</tt> que és l'equivalent al <tt>loadlin.exe</tt> de tota la vida, però per windows mobile, el kernel, el initrd i un autorun per a que quan insertem la tarjeta al windows mobile automàticament carregui linux. A la segona partició amb format EXT3 descomprimirem tota l'estructura de directoris del linux que posteriorment arrancarem.

<!--more-->

El procés per fer això que us he explicat és el següent:

Creem les 2 particions:

```
# cfdisk /dev/sda /dev/sda1: 10Mb tipo FAT16 (06) /dev/sda2: 54Mb tipo Linux
```

Formatejem amb FAT la primera partició i amb ext3 la segona:

```
s0 ~ # mkfs.vfat /dev/sda1 mkfs.vfat 2.11 (12 Mar 2005) s0 ~ # mkfs.ext3 /dev/sda2 mke2fs 1.38 (30-Jun-2005)
```

Montem les dos particions que hem creat:

```
s0 ~ # mkdir /mnt/sd0 s0 ~ # mkdir /mnt/sd1 s0 ~ # mount -t vfat /dev/sda1 /mnt/sd0 s0 ~ # mount -t ext3 /dev/sda2 /mnt/sd1
```

Creem la estructura de directoris necessaria sobre les dos particions:

```
s0 ~ # cd /mnt/sd0 s0 /mnt/sd0 # mkdir linux s0 /mnt/sd0 # cd linux s0 /mnt/sd0/linux # wget http://gnulinux.biz/files/blueangel/sd/linux/haret.exe s0 /mnt/sd0/linux # wget http://gnulinux.biz/files/blueangel/sd/linux/zImage-2.6.12 s0 /mnt/sd0/linux # wget http://gnulinux.biz/files/blueangel/sd/linux/initrd-2.6.12-hh2.gz s0 /mnt/sd0/linux # wget http://gnulinux.biz/files/blueangel/sd/linux/startup.txt s0 /mnt/sd0/linux # ls haret.exe initrd-2.6.12-hh2.gz startup.txt zImage-2.6.12 s0 /mnt/sd0/linux # cd .. s0 /mnt/sd0 # mkdir 2577 s0 /mnt/sd0 # cd 2577/ s0 /mnt/sd0/2577 # wget http://gnulinux.biz/files/blueangel/sd/2577/autorun.exe s0 /mnt/sd0/2577 # cd /tmp s0 /tmp # wget http://gnulinux.biz/files/blueangel/sd/linux/gpe-ba.tar.bz s0 /tmp # cd /mnt/sd1 s0 /mnt/sd1 # tar jxvfp /tmp/gpe-ba.tar.bz
```

Així es com quedaria la SD de 64Mb després del procés:

```
# df -h Filesystem Size Used Avail Use% Mounted on /dev/sda1 9.5M 6.4M 3.2M 67% /mnt/sd0 /dev/sda2 49M 42M 5.3M 89% /mnt/sd1
```

Finalment, ja podem desmontar la SD i posar-la a la PDA per veure com arranca Linux:

```
s0 ~ # sync s0 ~ # umount /dev/sda1 s0 ~ # umount /dev/sda2
```

Referències:

- [http://www.handhelds.org/moin/moin.cgi/BlueAngel](http://www.handhelds.org/moin/moin.cgi/BlueAngel)
- [http://wiki.xda-developers.com/index.php?pagename=BlueangelResearch](http://wiki.xda-developers.com/index.php?pagename=BlueangelResearch)
- [http://gnulinux.biz/files/blueangel/sd/](http://gnulinux.biz/files/blueangel/sd/)
- [http://www.linuxdevices.com/articles/AT7937511405.html](http://www.linuxdevices.com/articles/AT7937511405.html)
- <tt>#htc-blueangel</tt> a irc.freenode.net ([Logs](http://ibot.rikers.org/%23htc-blueangel/))
