---
layout: post
title: 'EncFS: sistema de fitxers encriptat a espai d''usuari'
date: 2005-01-11 11:21:42.000000000 +01:00
type: post
categories:
- linux
- security
tags: []
meta:
permalink: "/2005/01/11/encfs-sistema-de-fitxers-encriptat-a-espai-dusuari/"
---
[EncFS](http://encfs.sourceforge.net/) és un sistema de fitxers encriptat que treballa a espai d'usuari sense cap permís especial utilitzant [fuse](http://fuse.sourceforge.net/) per a proporcionar la interface del sistema de fitxers.

Permet encriptar les dades importants per a protegir-les d'atacs off-line (robatori de portàtil o backups). EncFS, a diferència d'altres sistemes de fitxers no necessita reservar un espai de disc per que no treballa amb un _block device_ sinó que treballa per separat sobre cadascun dels fitxers que introduïm al EncFS.

Per instal·lar-lo amb [Gentoo](http://www.gentoo.org) haurem de fer el següent:

```
# echo "sys-fs/encfs ~x86" \>\>/etc/portage/package.keywords # echo "dev-libs/rlog ~x86" \>\>/etc/portage/package.keywords # emerge -pv encfs These are the packages that I would merge, in order: Calculating dependencies ...done! [ebuild N] dev-libs/rlog-1.3.5 510 kB [ebuild N] sys-fs/fuse-1.4 124 kB [ebuild N] sys-fs/encfs-1.1.11 498 kB
```

Fixeu-vos que EncFS depén de fuse i rlog. Tant EncFS com rlog estan _Masked_ per tant haurem d'afegir-los al _package.keywords_ si treballem amb estable. Després només cal instalar encfs amb la comanda `emerge`.

Aquesta és la manera de crear un nou sistema de fitxers encriptat:

```
pau@s0~$ mkdir .cryptraw pau@s0~$ mkdir crypt pau@s0~$ encfs ~/.cryptraw/ ~/crypt/ Creating new encrypted volume. Please choose from one of the following options: enter "x" for expert configuration mode, enter "p" for pre-configured paranoia mode, anything else, or an empty line will select standard mode. ?\> p [...]
```

Totes les dades que posem dintre de ~/crypt/ es guardaran xifrades a ~/.cryptraw/.  
Si ens fixem com esta muntat, utilitza un descriptor de fitxer especial:

```
$ mount |grep crypt /proc/fs/fuse/dev on /home/pau/crypt type fuse (rw,nosuid,nodev,user=pau)
```

Per a desmuntar-lo haurem de fer un `fusermount -u ~/crypt/`, llavors només quedaran les dades xifrades a ~/.cryptraw/ i quedarà buit el directori ~/crypt/.

