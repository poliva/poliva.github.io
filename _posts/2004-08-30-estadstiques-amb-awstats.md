---
layout: post
title: Estadístiques amb awstats
date: 2004-08-30 21:39:27.000000000 +02:00
type: post
categories:
- linux
- pofHQ
tags: []
meta:
  tags: ''
permalink: "/2004/08/30/estadstiques-amb-awstats/"
---
Avui m'he configurat el [awstats](http://awstats.sourceforge.net/) per a generar estadístiques del servidor web, fins ara sempre havía utilitzat el [webalizer](http://www.mrunix.net/webalizer/), però fa molt de temps que no l'actualitzen (des-de el 2002!) i no té aquestes _cool features_ que te l'awstats com per exemple mostrar-te l'últim cop que t'ha visitat el googlebot :)

El procés d'instal·lació en gentoo ha estat una mica tediós, aixi que anoto aquí els passos que he seguit per si algún dia tinc que tornar-ho a fer, o per si li pot ser útil a algú. Per a fer-ho m'he basat amb les indicacions de Sevein en [aquest post](http://forums.gentoo.org/viewtopic.php?t=181288) dels fòrums de Gentoo.

```
# emerge awstats # cp /usr/share/webapps/awstats/6.1/postinst-en.txt /etc/apache2/conf/awstats.conf
```

Afegeixo això al final del fitxer, per a que em demane password al accedir a les estadístiques:

```
\<Directory "/usr/share/webapps/awstats/6.1/hostroot/cgi-bin/"\> Options None AllowOverride None Order allow,deny Allow from all AuthUserFile .htpasswd AuthName "Private Section" AuthType Basic \<Limit GET\> require user admin \</Limit\> \</Directory\>
```

```
# echo "Include conf/awstats.conf" \>\> /etc/apache2/conf/apache2.conf # cp /etc/awstats/awstats.model.conf /etc/awstats/awstats.mydomain.conf # vim /etc/awstats/awstats.mydomain.conf
```

jo he realitzat els següents canvis:

```
\< LogFile="/var/log/apache/access\_log" \> LogFile="/var/log/apache2/access\_log" \< SiteDomain="localhost" \> SiteDomain="pof.eslack.org" \< HostAliases="localhost 127.0.0.1 REGEX[myserver.com$]" \> HostAliases="localhost 127.0.0.1 REGEX[pofhq.net$] 192.168.1.1 s0" \< DNSLookup=2 \> DNSLookup=1 \< DirData="." \> DirData="/home/httpd/awstats\_tmp" \< DirCgi="/cgi-bin/awstats" \> DirCgi="/awstats" \< DirIcons="/awstats/icons" \> DirIcons="/awstatsicons"
```

Creem el directori `/home/httpd/awstats_tmp` i li donem permisos per a que pugui accedir l'usuari apache.

Ja podem generar les estadístiques:

```
# awstats\_updateall.pl now -awstatsprog=/usr/share/webapps/awstats/6.1/hostroot/cgi-bin/awstats.pl -configdir=/etc/awstats/
```

Això últim ho podem posar en un cron per a que les estadístiques es vagin actualitzant periòdicament. Per accedir a les estadístiques accedim a la url `http://www.mydomain.com/awstats/awstats.pl?config=mydomain`

Si veiem errors degut al ebuild de Gentoo que posa alguns permisos de forma incorrecta, seguim amb aquests pasos:

```
# cd /usr/share/webapps/awstats/6.1/hostroot/cgi-bin/ # chmod 755 lang/ lib/ plugins/ # cd /usr/share/webapps/awstats/6.1/htdocs/ # chmod 755 icon js css
```

I això és tot, awstats ja hauría d'estar funcionant com un cristal :)

