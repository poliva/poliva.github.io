---
layout: post
title: Gentoo Tips (II)
date: 2005-10-17 19:30:38.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: ''
permalink: "/2005/10/17/gentoo-tips-ii/"
---
![Gentoo]({{ site.baseurl }}/assets/images/2005/10/gentoo.png)

Gracies a en [Marc](http://www.noctambuls.net) m'he enterat de la existència de dos utilitats _must-have_ per als administradors de sistemes [Gentoo Linux](http://www.gentoo.org): **[Yacleaner](http://gentoo.org.mx/yacleaner/)**, per eliminar ebulds antics del sistema de paquets, i **module-rebuild** , un script que s'encarrega de reinstal·lar els mòduls del nucli que hem afegit al sistema quan canviem de kernel. El [post original del Marc](http://www.noctambuls.net/node/50) explica amb mes detall el que us resumeixo aquí:

```
s0 ~ # emerge sys-kernel/module-rebuild s0 ~ # module-rebuild module-rebuild [options] action [category/package] Version: 0.5 Where options are: -X - Emerge based on package names, not exact versions. -C - Disable all coloured output. Where action is one of: add - Add package to moduledb. del - Delete a package from moduledb. toggle - Toggle auto-rebuild of Package. list - List packages to auto-rebuild. rebuild - Rebuild packages. populate - Populate the database with any packages which currently install drivers into the running kernel. s0 ~ # module-rebuild list \*\* Packages which I will emerge are: =net-misc/zaptel-1.0.9\_p1-r1 =sys-fs/fuse-2.3.0
```

I un altre [article interessant de l'Arnau Bria](http://www.assl-site.net/article.php?story=20050914175455941) a la [ASSL](http://www.assl-site.net/) que explica com fer **bonding** amb dues interficies de xarxa amb Gentoo, així de simple:

- Kernel: `CONFIG_BONDING=m`
- `emerge ifenslave`
- `echo "bonding miimon=100 mode=1" >> /etc/modules.autoload.d/kernel-2.6`
- A `/etc/conf.d/net` afegir `iface_bond0="192.168.1.1 netmask 255.255.255.0 broadcast 192.168.1.255"` i comentar les entrades de eth0, eth1...
- `ln -sf /etc/init.d/net.eth0 /etc/init.d/net.bond0`
- `echo "ifenslave bond0 eth0 eth1" >> /etc/conf.d/local.start`
- `rc-update del net.eth0 && rc-update del net.eth1 && rc-update add net.bond0 default`
