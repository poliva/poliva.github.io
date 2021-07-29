---
layout: post
title: Flaixejar un wrt54g amb boot_wait per tftp
date: 2004-12-10 02:45:45.000000000 +01:00
type: post
categories:
- gadgets
- linux
- wireless
tags: []
meta:
permalink: "/2004/12/10/flaixejar-un-wrt54g-amb-boot_wait-per-tftp/"
---
Avui m'he vist en la necessitat de flaixejar un Linksys wrt54g per tftp, buscant a google hi ha 40.000 pàgines que expliquen com fer-ho, però cadascú ho explica d'una manera, aquí va el procés que jo he seguit basant-me en [aquestes instruccions](http://www.openwrt.org/temp/00-WARNING.TXT).

Partim de la base de que tenim el bootwait activat al linksys, sino tot això no servirà de res. Per activar el bootwait s'han d'executar les seguents comandes:

`
nvram set boot_wait="on"
nvram commit
`

D'aquesta manera si algún dia no ens arranca el firmware que tinguem instal·lat el linksys s'esperarà 5 segons al arrancar (amb el led de "diag" o de "power" parpadejant, segons el model) amb un servidor de tftp esperant que li enviem una nova imatge.

Per enviar-li la imatge he utilitzat el [atftp](ftp://ftp.mamalinux.com/pub/atftp/). Primer connectem el linksys directament a la tarjeta de red amb un cable creuat, seguidament (sense endollar el linksys a la corrent) executem les següents comandes:

```
# atftp tftp\> connect 192.168.1.1 tftp\> verbose Verbose mode on. tftp\> trace Trace mode on. tftp\> timeout 3 tftp\> status Connected: 192.168.1.1 port 69 Mode: octet Verbose: on Trace: on Options tsize: disabled blksize: disabled timeout: disabled multicast: disabled Last command: timeout 3
```

a continuació endollem el linksys a la corrent, i quan la llum de diag (o power) comenci a parpadejar enviem el fitxer amb la comanda '`put code.bin`', on code.bin ( **s'ha de dir aixi!** ) és el firmware que volem instal·lar; jo he posat un [hyperwrt](http://www.hyperdrive.be/hyperwrt/). Si hem fet el procés prou ràpid (només tenim uns 3-4 segons de marge) veurem que el fitxer s'envia:

```
tftp\> put code.bin sent WRQ \<file: code.bin, mode: octet \<\>\> [...] sent DATA \<block: 6595, size: 0\> received ACK \<block: 6595\>
```

i al cap d'uns minuts, ja tindrem el nostre linksys flashejat de nou :)

