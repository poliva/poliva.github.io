---
layout: post
title: Asterisk i MySQL
date: 2005-09-11 03:55:33.000000000 +02:00
type: post
categories: []
tags: []
meta:
  tags: asterisk mysql asterisk-stat
permalink: "/2005/09/11/asterisk-i-mysql/"
---
A continuació teniu els passos que s'han de seguir per a que [Asterisk](http://www.asterisk.org/) guarde els logs sobre una [MySQL](http://www.mysql.com/). Per fer-ho amb [Gentoo](http://www.gentoo.org/) s'ha de compilar l'Asterisk amb `USE="mysql"`.

Crear el fitxer `/etc/asterisk/cdr_mysql.conf` amb el següent contingut.

```
; Note - if the database server is hosted on the same machine as the ; asterisk server, you can achieve a local Unix socket connection by ; setting hostname=localhost ; ; port and sock are both optional parameters. If hostname is specified ; and is not "localhost", then cdr\_mysql will attempt to connect to the ; port specified or use the default port. If hostname is not specified ; or if hostname is "localhost", then cdr\_mysql will attempt to connect ; to the socket file specified by sock or otherwise use the default socket ; file. ; [global] hostname=localhost dbname=cdr password=yourpassword user=asteriskuser ;port=3306 ;sock=/tmp/mysql.sock ;userfield=1
```

Crear la base de dades `cdr` amb la taula `cdr`. Només cal copiar les següents senténcies SQL en un fitxer de text:

```
CREATE DATABASE cdr; GRANT INSERT ON cdr.\* TO asteriskuser@localhost IDENTIFIED BY 'yourpassword'; USE cdr; CREATE TABLE cdr ( uniqueid varchar(32) NOT NULL default '', userfield varchar(255) NOT NULL default '', accountcode varchar(20) NOT NULL default '', src varchar(80) NOT NULL default '', dst varchar(80) NOT NULL default '', dcontext varchar(80) NOT NULL default '', clid varchar(80) NOT NULL default '', channel varchar(80) NOT NULL default '', dstchannel varchar(80) NOT NULL default '', lastapp varchar(80) NOT NULL default '', lastdata varchar(80) NOT NULL default '', calldate datetime NOT NULL default '0000-00-00 00:00:00', duration int(11) NOT NULL default '0', billsec int(11) NOT NULL default '0', disposition varchar(45) NOT NULL default '', amaflags int(11) NOT NULL default '0' );
```

I seguidament executar la comanda:

```
mysql -u root -p \< nom\_del\_fitxer
```

Reiniciem l'asterisk i a partir d'ara les trucades es guardaran a la mysql.

També podeu instal·lar [asterisk-stat](http://www.areski.net/areski/index.php?option=com_content&task=view&id=22&Itemid=54), un programa amb php que serveix per accedir als <acronym title="Call Data Records">CDR</acronym> de forma gràfica, aquí podeu veure un exemple:

[![asterisk-stat]({{ site.baseurl }}/assets/images/2005/09/thumb-asteriskstat.jpg)](/archives/images/asteriskstat.jpg)

