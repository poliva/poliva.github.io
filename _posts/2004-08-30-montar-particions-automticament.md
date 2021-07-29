---
layout: post
title: Montar particions automàticament
date: 2004-08-30 17:18:43.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: ''
permalink: "/2004/08/30/montar-particions-automticament/"
---
Això de montar les particions automàticament era un tema que em volia mirar, ara que [m'he comprat el HD USB](/blog/2004/08/25/54/) ja que trovo que és més còmode que montar-les a mà, la veritat es que mai m'hagués pensat que fos tan fàcil, aquí la recepta que m'ha passat l'Esteve, per als que ho volgueu montar en dos minuts:

Al kernel, heu d'entrar a l'apartat "_File Systems_" i sel·leccionar l'opció "_Kernel automounter version 4 support_". Desprès s'han d'instalar les _user space tools_, si utilitzeu Gentoo com jo, només cal fer això:

```
# emerge autofs
```

Llavors editem el fitxer `/etc/autofs/auto.master` per especificar sobre quin directori funcionarà l'autofs, jo l'he fet de la següent manera:

```
/mnt /etc/autofs/auto.mnt --timeout 10
```

Amb la opció _timeout_ especifiquem el temps que volem que tarde en desmontar la partició automàticament.

Llavors editem el fitxer `/etc/autofs/auto.mnt` i especifiquem les unitats que volem que ens monte automàticament i amb quines opcions. A mí m'ha quedat aixi:

```
cdrom -fstype=iso9660,ro,uid=5000,gid=100 :/dev/cdrom dos -fstype=vfat,uid=5000,gid=100 :/dev/hda1 usb -fstype=vfat,uid=5000,gid=100 :/dev/sda2 camara -fstype=vfat,uid=5000,gid=100 :/dev/sda1
```

Finalment, només cal arrancar el dimoni d'autofs amb `/etc/init.d/autofs start`.

A partir d'ara quan faig un `ls -l /mnt/usb` em monta automàticament el HD USB i me'l desmonta després de 10 segons sense utilitzar-lo. :)

