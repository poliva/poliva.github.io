---
layout: post
title: Recuperant una particio ext3
date: 2005-09-20 01:04:49.000000000 +02:00
type: post
categories: []
tags: []
meta:
  tags: ext3 filesystem partition fsck recovery
permalink: "/2005/09/20/recuperant-una-particio-ext3/"
---
Últimament [estic gafat](/blog/2005/09/04/el-disc-ha-petat/). Avui ha estat tot el dia el servidor de pofHQ parat degut a un _error humà_ al fer un backup amb [Partimage](http://www.partimage.org/) sobre una partició montada. El programa diu que és perillós fer-ho, però tot i així he fet una prova sobre `/tmp` i ha funcionat, així que després he seguit amb el `/var`, que ha fallat i m'ha destroçat la partició.

Després de provar `e2fsck` amb totes les opcions possibles em pensava que hauría de reinstalar (havía perdut la metadata del <tt>portage</tt>), però finalment he aconseguit recuperar la partició :)

L'error que donava al fer el `fsck` era el següent:

```
Group descriptors look bad... trying backup blocks... Block bitmap for group 0 is not in group. (block xxxxxxxx) Relocate? Inode table for group 0 is not in group. (block xxxxxxxx) WARNING: SEVERE DATA LOSS POSSIBLE. Relocate? [...] e2fsck\_read\_bitmaps: illegal bitmap block(s) for /dev/hda3
```

Semblava com si el `fsck` no tingués efecte, ja que era impossible reparar-la partició, sempre tornava a donar els mateixos errors. El procés que he següit per sol·lucionar-ho ha estat més o menys el següent:

Primer he fet un backup en _raw_ de la partició utilitzant `dd`:

```
# dd if=/dev/hda3 of=/backup/particio-hda3
```

Seguidament he formatejat la partició amb <tt>ext2</tt> (era <tt>ext3</tt>, però el journaling estava corrupte) passant-li la opció `-S` a `mke2fs`, per a que reinicialitze el superblock i els descriptors de grup sense tocar la taula de inodes ni els bitmaps d'inode i de block, d'aquesta manera no es perden les dades.

```
# mkfs.ext2 -S /dev/hda3
```

NOTA: en cas de tenir un tamany de block especificat manualment al formatejar la partició, cal especificar-lo aquí també amb la opció `-b`.

Després d'això he pogut fer un `fsck.ext2 -v -y /dev/hda3` i he recuperat les dades, però sobre una partició <tt>ext2</tt> en lloc de <tt>ext3</tt>. La he montat i he fet un tar de tot, per a tornar-la a formatejar amb <tt>ext3</tt> i desfer el tar per recuperar les dades originals sobre <tt>ext3</tt>.

Per sort no s'ha perdut rés i no ha fet falta reinstal·lar, ara tot torna a funcionar amb normalitat. :)

