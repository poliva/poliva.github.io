---
layout: post
title: El disc ha petat
date: 2005-09-04 00:23:12.000000000 +02:00
type: post
categories:
- linux
- pofHQ
tags: []
meta:
  tags: raid linux linux-raid
permalink: "/2005/09/04/el-disc-ha-petat/"
---
<p>Fa un temps ja va fer <a href="/blog/2005/08/02/em-falla-un-disc-del-raid1/">un amago</a>, i ara finalment ha petat del tot. Un dels dos discs de 120Gb que tenia funcionant com a RAID 1 per al <code>/home</code> del servidor.</p>
<p>Vaig adonar-me'n perquè el <a href="http://cgi.cse.unsw.edu.au/~neilb/mdadm">mdadm</a> em va avisar per mail, i al fer un <code>dmesg</code> vaig veure això, que ja no em va fer gens de bona espina:</p>
<pre>
hdg: dma_intr: status=0x51 { DriveReady SeekComplete Error }
hdg: dma_intr: error=0x40 { UncorrectableError }, LBAsect=280332, sector=280328
ide: failed opcode was: unknown
end_request: I/O error, dev hdg, sector 280328
raid1: Disk failure on hdg, disabling device.
        Operation continuing on 1 devices
raid1: hdg: rescheduling sector 280304
raid1: hde: redirecting sector 280304 to another mirror
RAID1 conf printout:
 --- wd:1 rd:2
 disk 0, wo:0, o:1, dev:hde
 disk 1, wo:1, o:0, dev:hdg
</pre>
<p>Finalment he acabat canviant els dos discs de 120Gb per dos discs de 200Gb, però el procés per fer-ho sense perdre cap dada ha estat una odissea...</p>
<p><!--more--></p>
<p>Primer vaig eliminar el disc que fallava del array:</p>
<pre>
s0 ~ # mdadm --manage /dev/md0 --remove /dev/hdg
mdadm: hot removed /dev/hdg
s0 ~ # dmesg
RAID1 conf printout:
 --- wd:1 rd:2
 disk 0, wo:0, o:1, dev:hde
md: unbind&lt;hdg&gt;
md: export_rdev(hdg)
</pre>
<p>Vaig particionar-lo amb <tt>ext3</tt> i li vaig fer un <code>fsck</code> amb les opcions per comprovar <em>bad blocks</em>, i aquest cop en va trobar 24:</p>
<pre>
s0 ~ # fsck.ext3 -c -f /dev/hdg1
e2fsck 1.38 (30-Jun-2005)
Checking for bad blocks (read-only test): done                        2088
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information

/dev/hdg1: ***** FILE SYSTEM WAS MODIFIED *****

      11 inodes used (0%)
       0 non-contiguous inodes (0.0%)
         # of inodes with ind/dind/tind blocks: 0/0/0
  503791 blocks used (1%)
      24 bad blocks
       0 large files

       0 regular files
       2 directories
       0 character device files
       0 block device files
       0 fifos
       0 links
       0 symbolic links (0 fast symbolic links)
       0 sockets
--------
2 files

Llavors vaig posar un HD de 200Gb amb el de 120 que ja hi havia, i em va reconstruir el array correctament, així que vaig treure el de 120 i vaig posar un altre de 200Gb, però aquest cop no es reconstruïa, el error que em va donar era:

```
No super block found on /dev/md0 (Expected magic xxxxxxxx, got 00000000)
```

Només vaig haver de canviar l'ordre dels dispositius a `/etc/raidtab` per a poder-lo reconstruir de nou.

Un cop estava el raid funcionant amb els dos nous discs de 200Gb, el tamany continuava sent de 120Gb, així que vaig haver de canviar-li el tamany al raid, i posteriorment al sistema de fitxers. Per fer-ho vaig consultar el manual del `mdadm`:

> **-z, --size=**
> 
> Amount (in Kibibytes) of space to use from each drive in RAID1/4/5/6. This must be a multiple of the chunk size, and must leave about 128Kb of space at the end of the drive for the RAID superblock. If this is not specified (as it normally is not) the smallest drive (or partition) sets the size, though if there is a variance among the drives of greater than 1%, a warning is issued.
> 
> This value can be set with --grow for RAID level 1/4/5/6. If the array was created with a size smaller than the currently active drives, the extra space can be accessed using --grow.
> 
> [...]
> 
> **GROW MODE**
> 
> The GROW mode is used for changing the size or shape of an active array. For this to work, the kernel must support the necessary change. Various types of growth may be added during 2.6 development, possibly including restructuring a raid5 array to have more active devices.
> 
> Currently the only support available is to change the "size" attribute for arrays with redundancy, and the raid-disks attribute of RAID1 arrays.
> 
> Normally when an array is build the "size" it taken from the smallest of the drives. If all the small drives in an arrays are, one at a time, removed and replaced with larger drives, then you could have an array of large drives with only a small amount used. In this situation, changing the "size" with "GROW" mode will allow the extra space to start being used. If the size is increased in this way, a "resync" process will start to make sure the new parts of the array are synchronised.
> 
> Note that when an array changes size, any filesystem that may be stored in the array will not automatically grow to use the space. The filesystem will need to be explicitly told to use the extra space.

La opció `--grow` combinada amb `--size` semblava la ideal, així que vaig calcular el nou tamany del array, `fdisk` ens dona el tamany del disk en bytes:

```
Disk /dev/hdg: 203.9 GB, 203928109056 bytes
```

Segons el <tt>man</tt> del `mdadm`, hem d'especificar el tamany en [Kibibytes](http://en.wikipedia.org/wiki/Kibibyte) per tant 203928109056/1024 == 199148544. A aquest nombre li hem de restar 128Kb per al superblock del RAID. El nombre que queda és 199148544-128 == 199148416, que és múltiple del tamany de _chunk_ (64bytes).

Amb aquests càlculs tot semblava [perfecte](http://www.google.es/search?q=199148416+kibibytes+to+gigabytes), així que vaig executar la comanda següent:

```
s0 ~ # mdadm --grow /dev/md0 --size 199148416
```

Però en lloc de augmentar el tamany del array a 200Gb el va reduir a 20Gb!!! Per sort guardava el disc del raid original de 120Gb intacte, però no va fer falta ja que després de rebotar el `fsck` em va arreglar el tamany del disc i no vaig perdre cap dada. Crec que el problema està en que no s'ha d'especificar el tamany en Kibibytes, sinó en bytes i la pàgina <tt>man</tt> està incorrecta. He enviat [un mail](http://marc.theaimsgroup.com/?l=linux-raid&m=112592074611142&w=2) a la llista de <tt>linux-raid</tt> per reportar-ho, però de moment encara no he rebut resposta.

Finalment ja tenia el array a 200Gb, però la partició <tt>ext3</tt> que contenia continuava sent de 120Gb, així que vaig haver de canviar-li el tamany a la partició, aquests són els passos que vaig seguir per fer-ho i comprovar que no hi hagués cap error:

```
s0 ~ # fsck.ext3 -fv /dev/md0 s0 ~ # resize2fs -p /dev/md0 s0 ~ # fsck.ext3 -fv /dev/md0
```

I ara ja tinc el raid1 amb dos discs de 200Gb, esperem que aquest cop aguanten més que els de 120 :).

Aprofitant la pèrdua de <tt>uptime</tt> també li he posat 1Gb de RAM més al server (ara té 1,5Gb), li he canviat un ventilador lateral de 8" que s'havia fet malbé, li'n he afegit un de nou al darrera sota de la font i li he canviat tots els cables IDE per mangueres redones d'aquestes típiques de _modding_.

