---
layout: post
title: Dependències inverses amb Gentoo
date: 2005-01-12 16:29:21.000000000 +01:00
type: post
categories:
- linux
tags: []
meta:
permalink: "/2005/01/12/dependencies-inverses-amb-gentoo/"
---
Molta gent es pregunta com saber les dependències inverses amb Gentoo. Les dependències inverses son els paquets que necessiten a un determinat paquet instal·lat. Per exemple, hem vist que [el EncFS té com a dependencia el fuse](/blog/2005/01/11/154/), per tant una dependència inversa de fuse és EncFS.

Això és útil quan volem eliminar un paquet, però no sabem si al fer-ho alguna altra cosa deixarà de funcionar... per que el que estem eliminant era una dependència inversa.

Per a veure les dependències inverses utilitzarem la comanda _qpkg_, inclosa en el paquet _gentoolkit_: ` qpkg -q -I paquet`

Posem alguns exemples:

```
$ **qpkg -q -I fuse** sys-fs/fuse-1.4 \* DEPENDED ON BY: encfs-1.1.11 $ **qpkg -q -I net-print/cups** net-print/cups-1.1.23\_rc1 \* DEPENDED ON BY: ghostscript-7.07.1-r7 samba-3.0.9-r1
```
