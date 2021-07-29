---
layout: post
title: Replicar una instal·lació de linux
date: 2005-06-09 21:28:52.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
permalink: "/2005/06/09/replicar-una-installacio-de-linux/"
---
Hi ha moltes maneres de fer una replica d'un Linux que ja tenim prèviament instal·lat, es pot fer utilitzant utilitats com dd, ghost, etc... a continuació us explicaré una forma fàcil i ràpida de fer-ho, deixant un CD de instal·lació que només calgui posar-lo i fer _siguiente, siguiente..._.

El mètode es basa en modificar el CD d'instal·lació de Gentoo, per a que quan arranque execute una serie de scripts que facin la feina de particionar i instal·lar per nosaltres.

El primer pas és fer un tarball de tot el sistema que volem replicar:

```
# tar cvfz /tmp/system.tgz /bin /boot /dev /etc /home /lib \ /mnt /opt /root /sbin /tmp /usr /var
```

Seguidament, baixem la ISO [x86-minimal](http://trumpetti.atm.tut.fi/gentoo/releases/x86/current/installcd/install-x86-minimal-2005.0.iso) (59Mb) de Gentoo.

El fitxer `livecd.squashfs` conté la imatge del sistema de fitxers que utilitza el CD d'instal·lació de Gentoo, si tenim suport de [squashfs](http://squashfs.sourceforge.net/) al kernel del nostre sistema podrem muntar-la directament, però com s'ha de parxejar el kernel és poc probable que tinguem suport.

La sol·lució més ràpida és tostar la ISO de Gentoo i arrancar des de CD, llavors ja podrem muntar el fitxer livecd.squashfs per extreure'n el contingut. Ho fem de la següent manera:

```
# mkdir /mnt/pof # mount -o loop -t squashfs /mnt/cdrom/livecd.squashfs /mnt/pof/
```

Llavors muntem el HD que estàvem replicant, i fem un tarball del squashfs que acabem de muntar al temporal:

```
# mkdir /mnt/pof2 # mount /dev/hda1 /mnt/pof2 (substituir hda1 per la partició corresponent) # tar cvfz /mnt/pof2/tmp/squash.tgz /mnt/pof
```

Un cop fet el tarball ja podem reiniciar amb el nostre sistema, de forma normal; si tot ha anat bé tindrem el fitxer squash.tgz al directori temporal.

Seguidament, descomprimim i modifiquem el sistema de fitxers que hem extret.

Jo he creat un directori `/etc/pof`, on he col·locat els següents scripts: [instalar.sh](/archives/files/instalar.sh) i [config.sh](/archives/files/config.sh).

Nota: Si no utilitzes discs SCSI, canvia `/dev/sda` per `/dev/hda` o el dispositiu adient.

Per a fer que s'executi el script de instal·lació un cop arrancat el sistema, afegim aquesta línia al final del fitxer `etc/conf.d/local.start`:

```
/etc/pof/instalar.sh
```

Aquest script crearà 2 particions de forma no interactiva, una per al sistema de fitxers i una per a la swap, descomprimirà el primer tarball que hem creat (system.tgz) i executarà el segon script (config.sh) dintre d'un chroot amb el sistema descomprimit, que s'encarregarà d'instal·lar el gestor d'arranc grub.

Un cop modificat el sistema del livecd de Gentoo, tornem a convertir-lo en un fitxer squashfs, per fer-ho necessitarem instal·lar squashfs-tools:

```
# mksquashfs /tmp/squash\_dir /tmp/livecd-custom.squashfs
```

Amb això generem el fitxer livecd.squashfs amb les nostres modificacions. Ara anem a crear el contingut del que serà el nostre CD d'instal·lació personalitzat:

```
# mkdir /tmp/gentoo-minimal /tmp/custom-cd # mount -o loop install-x86-minimal-2005.0.iso /tmp/gentoo-minimal/ # cd /tmp/custom-cd # cp -R /tmp/gentoo-minimal/\* . # cp /tmp/livecd-custom.squashfs /tmp/custom-cd/livecd.squashfs # cp /tmp/system.tgz /tmp/custom-cd/
```

Ja tenim el CD preparat, ara generem la imatge ISO utilitzant mkisofs:

```
# cd /tmp/custom-cd/ # mkisofs -o /tmp/x86-custom-linux.iso -R -V "Custom CD Install" \ -v -d -D -N -no-emul-boot -boot-load-size 4 -boot-info-table \ -b isolinux/isolinux.bin -c isolinux/isolinux.boot -A "Custom CD" .
```

Si tot ha anat bé tindrem la ISO al directori temporal, ara només queda tostar-la:

```
# cdrecord dev=ATAPI:0,0,0 fs=4096k -v -useinfo -speed=16 -dao \ -eject -pad -data "/tmp/x86-custom-linux.iso"
```
