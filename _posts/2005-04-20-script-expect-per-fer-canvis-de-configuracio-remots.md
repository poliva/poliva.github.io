---
layout: post
title: Script expect per fer canvis de configuració remots
date: 2005-04-20 16:27:38.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
permalink: "/2005/04/20/script-expect-per-fer-canvis-de-configuracio-remots/"
---
[Expect](http://expect.nist.gov/) és un llenguatge de programació interpretat pensat principalment per a facilitar-nos la feina a l'hora d'enviar dades a aplicacions que requereixen interactivitat per tal d'automatitzar tasques, els exemples típics serien sessions de telnet o ftp o aplicacions on hagem d'introduir un password per teclat.

A mi m'ha tocat fer un scriptillo que anés connectant a varies màquines per ssh, i executés una comanda dins de cada màquina, com veureu amb expect és molt fàcil :)

Partim d'un fitxer <acronym title="Comma Separated Values">CSV</acronym> on tindrem el llistat de IP's on hem de connectar amb els corresponents logins i passwords.

`list.csv`:

```
IP ; Login ; Passwd
```

Des-de un shell-script farem el bucle per totes les màquines per on vulguem passar, per exemple:

```
#!/bin/sh for linea in `cat list.csv`; do param1=`echo $linea |cut -f 1 -d ";"` param2=`echo $linea |cut -f 2 -d ";"` param3=`echo $linea |cut -f 2 -d ";"` ./set-config.expect $param1 $param2 $param3 done
```

Ara el fitxer amb expect:

`set-config.expect`:

```
#!/usr/bin/expect -f set force\_conservative 1 ;# set to 1 to force conservative mode even if ;# script wasn't run conservatively originally if {$force\_conservative} { set send\_slow {1 .001} proc send {ignore arg} { sleep .1 exp\_send -s -- $arg } } puts "\n" send\_user "\n============================\n" send\_user "$argv0 [lrange $argv 0 2]\n" send\_user "============================\n" spawn ssh -l [lrange $argv 1 1] [lrange $argv 0 0] expect "continue connecting (yes/no)\*" send -- "yes\r" expect "assword:\*" send -- "[lrange $argv 2 2]\r" send -- "/bin/ls\r" # or whatever you want... expect { timeout {exit} -gl "\[\*]\$\*" } puts "\n" exit
```

Fàcil, no? Si voleu fer alguna cosa una mica més currada sempre podeu consultar el [tutorial de expect](http://www.linuxlots.com/~barreiro/spain/expect/).

