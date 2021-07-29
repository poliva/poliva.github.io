---
layout: post
title: Upgrades al server de pofHQ
date: 2005-11-06 03:05:07.000000000 +01:00
type: post
categories:
- linux
- pofHQ
- security
tags: []
meta:
  tags: mysql apache gentoo upgrade mod_security
permalink: "/2005/11/06/upgrades-al-server-de-pofhq/"
---
Acabo d'actualitzar el servidor web ([apache](http://www.apache.org/)) i el servidor de BBDD ([MySQL](http://www.mysql.com/)) del servidor que hospeda aquestes pàgines, el _layout_ a disc de la configuració d'Apache sobre [Gentoo](http://www.gentoo.org) ha canviat una mica en la última versió estable, els passos per a fer l'upgrade están a la guía [Upgrading Apache (Gentoo)](http://www.gentoo.org/doc/en/apache-upgrading.xml), i també, l'upgrade de 4.0.x a 4.1.x de MySQL no és directe, els passos per fer l'upgrade están a la guía [Upgrade guide to MySQL 4.1.x (Gentoo)](http://www.gentoo.org/doc/en/mysql-upgrading.xml).

De pas he aprofitat per posar el [mod\_security](http://www.modsecurity.org/) a apache, amb Gentoo és molt simple d'instalar:

```
# emerge mod\_security
```

![mod_security]({{ site.baseurl }}/assets/images/2005/11/mod_security.png)

Un cop instalat s'ha de configurar al fitxer <tt>/etc/apache2/modules.d/99_mod_security.conf</tt>, on hi ha alguns exemples mínims. Per habilitar-lo cal afegir `-D SECURITY` a la variable `APACHE2_OPTS` del fitxer <tt>/etc/conf.d/apache2</tt>.

Si voleu saber més sobre mod\_security us recomano donar-li una ullada a la [presentació](http://www.isecauditors.com/downloads/present/WhatTheHack_2005_modsecurity.pdf) que Daniel Fernandez i Christian Martorella van fer a la WhatTheHack'05.

