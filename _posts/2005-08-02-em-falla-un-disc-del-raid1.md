---
layout: post
title: Em falla un disc del raid1
date: 2005-08-02 19:33:14.000000000 +02:00
type: post
categories:
- linux
- pofHQ
tags: []
meta:
  tags: ''
permalink: "/2005/08/02/em-falla-un-disc-del-raid1/"
---
<p><strong>Panic! Terror! Aaaghh!</strong></p>
<p>El dimecres passat vaig rebre un email del <em>daemon</em> que controla el raid 1 de dos discs de 120 Gb que tinc montat com a <code>/home</code> del servidor de casa, on està allotjat aquest blog, informant-me de que un dels discs havia fallat:</p>
<pre>
From: 	mdadm monitoring
Subject: 	Fail event on /dev/md0:s0
Date: 	Wed, 27 Jul 2005 23:12:17 +0200

This is an automatically generated mail message from mdadm
running on s0

A Fail event had been detected on md device /dev/md0.


Faithfully yours, etc.
</pre>
<p>Fins avui no havia tingut temps de mirar-m'ho amb calma... i sorpresa la mia, estic funcionant només amb un disc!! Això vol dir que si peta el disc, em quedo sense totes les dades que tinc al <code>/home</code>...  ara sortiré corrents cap a Rda. St. Antoni a comprar un disc, aixi que el servidor estarà parat aquesta nit mentre el canvii... aprofitaré el <em>downtime</em> per posar-li també una unitat de CD (ara no en té) i una altra X100P per connectar-hi un trunk.</p>
<p>Aquí deixo constància dels passos per comprovar el raid.... com es pot veure hi ha un disk que falla (<code>/dev/hdg</code>):</p>
<pre>
s0 ~ # lsraid -D -l -a /dev/md0
[dev 33, 0] /dev/hde:
        md version              = 0.90.0
        superblock uuid         = 2484EF77.198495F3.E0BD280C.A13A89EF
        md minor number         = 0
        created                 = 1068606386 (Wed Nov 12 04:06:26 2003)
        last updated            = 1123003057 (Tue Aug  2 19:17:37 2005)
        raid level              = 1
        chunk size              = 4 KB
        apparent disk size      = 120060800 KB
        disks in array          = 1
        required disks          = 2
        active disks            = 1
        working disks           = 1
        failed disks            = 1
        spare disks             = 0
        position in disk list   = 0
        position in md device   = 0
        state                   = good

s0 ~ # lsraid -D -a /dev/md0 -d /dev/hdg
[dev 33, 0] /dev/hde:
        md device       = [dev 9, 0] /dev/md0
        md uuid         = 2484EF77.198495F3.E0BD280C.A13A89EF
        state           = good

[dev 34, 0] /dev/hdg:
        old md device   = [dev 9, 0]
        old md uuid     = 2484EF77.198495F3.E0BD280C.A13A89EF
        state           = unknown
</pre>
<p><!--more--></p>
<p><strong>Update</strong>: He reiniciat el server sense cambiar cap disc, i he vist que el raid arrancava només amb un disc (<code>/dev/hde</code>), però l'altre disc l'ha detectat correctament la BIOS y el sistema operatiu i el <code>mdadm</code> m'ha informat de que el raid està degradat, es a dir, la informació dels dos discs no està sincronitzada:</p>
<pre>
s0 ~ # mdadm --detail /dev/md0
/dev/md0:
        Version : 00.90.01
  Creation Time : Wed Nov 12 04:06:26 2003
     Raid Level : raid1
     Array Size : 120060800 (114.50 GiB 122.94 GB)
    Device Size : 120060800 (114.50 GiB 122.94 GB)
   Raid Devices : 2
  Total Devices : 1
Preferred Minor : 0
    Persistence : Superblock is persistent

    Update Time : Tue Aug  2 21:17:24 2005
          State : clean, degraded
 Active Devices : 1
Working Devices : 1
 Failed Devices : 0
  Spare Devices : 0

           UUID : 2484ef77:198495f3:e0bd280c:a13a89ef
         Events : 0.16568598

    Number   Major   Minor   RaidDevice State
       0      33        0        0      active sync   /dev/hde
       1       0        0        -      removed
</pre>
<p>Així que com aparentment el disc no està cascat, he forçat la reconstrucció del raid:</p>
<pre>
s0 ~ # raidhotadd /dev/md0 /dev/hdg

s0 ~ # dmesg
md: bind&lt;hdg&gt;
RAID1 conf printout:
 --- wd:1 rd:2
 disk 0, wo:0, o:1, dev:hde
 disk 1, wo:1, o:1, dev:hdg
.&lt;6&gt;md: syncing RAID array md0
md: minimum _guaranteed_ reconstruction speed: 1000 KB/sec/disc.
md: using maximum available idle IO bandwith (but not more than 200000 KB/sec)
 for reconstruction.
md: using 128k window, over a total of 120060800 blocks.

s0 ~ # mdadm --detail /dev/md0
/dev/md0:
        Version : 00.90.01
  Creation Time : Wed Nov 12 04:06:26 2003
     Raid Level : raid1
     Array Size : 120060800 (114.50 GiB 122.94 GB)
    Device Size : 120060800 (114.50 GiB 122.94 GB)
   Raid Devices : 2
  Total Devices : 2
Preferred Minor : 0
    Persistence : Superblock is persistent

    Update Time : Tue Aug  2 21:41:43 2005
          State : clean, degraded, recovering
 Active Devices : 1
Working Devices : 2
 Failed Devices : 0
  Spare Devices : 1

 Rebuild Status : 12% complete

           UUID : 2484ef77:198495f3:e0bd280c:a13a89ef
         Events : 0.16569389

    Number   Major   Minor   RaidDevice State
       0      33        0        0      active sync   /dev/hde
       1       0        0        -      removed

       2      34        0        1      spare rebuilding   /dev/hdg

s0 ~ # lsraid -D -a /dev/md0 -d /dev/hdg
[dev 34, 0] /dev/hdg:
        md device       = [dev 9, 0] /dev/md0
        md uuid         = 2484EF77.198495F3.E0BD280C.A13A89EF
        state           = spare

[dev 33, 0] /dev/hde:
        md device       = [dev 9, 0] /dev/md0
        md uuid         = 2484EF77.198495F3.E0BD280C.A13A89EF
        state           = good
</pre>
<p>De moment porta un 12% i sembla que va tot bé... de tota manera he comprat un Maxtor de 200 Gb (7200 RPM 8Mb) per 82 euros (6 euros menys que el que em va costar <a href="/blog/2004/08/25/carcasa-per-a-hd-extern-usb/">fa gairebé un any</a> el mateix disc), que igualment posaré al server només per fer backups :P</p>
<p><strong>Update 2</strong>: El raid s'ha acabat de reconstruir correctament, suposo que només devía ser un fallo de sync i que el disc no està afectat.</p>
<pre>
s0 ~ # dmesg
md: md0: sync done.
RAID1 conf printout:
 ---
wd:2 rd:2 disk 0, wo:0, o:1, dev:hde disk 1, wo:0, o:1, dev:hdg s0 ~ # mdadm --detail /dev/md0 /dev/md0: Version : 00.90.01 Creation Time : Wed Nov 12 04:06:26 2003 Raid Level : raid1 Array Size : 120060800 (114.50 GiB 122.94 GB) Device Size : 120060800 (114.50 GiB 122.94 GB) Raid Devices : 2 Total Devices : 2 Preferred Minor : 0 Persistence : Superblock is persistent Update Time : Wed Aug 3 02:27:38 2005 State : clean Active Devices : 2 Working Devices : 2 Failed Devices : 0 Spare Devices : 0 UUID : 2484ef77:198495f3:e0bd280c:a13a89ef Events : 0.16578408 Number Major Minor RaidDevice State 0 33 0 0 active sync /dev/hde 1 34 0 1 active sync /dev/hdg s0 ~ # lsraid -D -l -a /dev/md0 [dev 33, 0] /dev/hde: md version = 0.90.0 superblock uuid = 2484EF77.198495F3.E0BD280C.A13A89EF md minor number = 0 created = 1068606386 (Wed Nov 12 04:06:26 2003) last updated = 1123028858 (Wed Aug 3 02:27:38 2005) raid level = 1 chunk size = 4 KB apparent disk size = 120060800 KB disks in array = 2 required disks = 2 active disks = 2 working disks = 2 failed disks = 0 spare disks = 0 position in disk list = 0 position in md device = 0 state = good [dev 34, 0] /dev/hdg: md version = 0.90.0 superblock uuid = 2484EF77.198495F3.E0BD280C.A13A89EF md minor number = 0 created = 1068606386 (Wed Nov 12 04:06:26 2003) last updated = 1123028928 (Wed Aug 3 02:28:48 2005) raid level = 1 chunk size = 4 KB apparent disk size = 120060800 KB disks in array = 2 required disks = 2 active disks = 2 working disks = 2 failed disks = 0 spare disks = 0 position in disk list = 1 position in md device = 1 state = good

