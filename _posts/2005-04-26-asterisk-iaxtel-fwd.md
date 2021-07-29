---
layout: post
title: Asterisk + IAXTel + FWD
date: 2005-04-26 23:20:32.000000000 +02:00
type: post
categories:
- linux
- voip
tags: []
meta:
permalink: "/2005/04/26/asterisk-iaxtel-fwd/"
---
Aquest cap de setmana he configurat [Asterisk](http://www.asterisk.org/) per connectar a [IAXTel](http://www.iaxtel.com/) i a [Free World Dialup](http://www.freeworlddialup.com/).

Ambdós són serveis gratuïts i funcionen utilitzant el protocol <acronym title="Inter Asterisk eXchange">IAX</acronym>2. El que pretenen és crear una xarxa d'usuaris connectats de manera que les trucades entre aquests usuaris siguin gratuïtes, per tant els dos serveis proporcionen números de telèfon, que només funcionen si tenim el nostre Asterisk connectat al seu servidor. La configuració, tot i que pot semblar trivial m'ha portat més mals de caps del que em podia imaginar inicialment, tot degut a que [IAXTel](http://www.iaxtel.com/) no funciona massa be i dòna _timeouts_ tota la estona, quan no pings molt elevats. Em sembla que l'han deixat de mantenir i de tant en tant funciona _de milacre_, així que us recomano que passeu de [IAXTel](http://www.iaxtel.com/) i utilitzeu [FWD](http://www.freeworlddialup.com/).

La gràcia d'aquests serveis, a mes de que permeten trucar gratis a altres membres del servei (que n'hi ha ben pocs, al menys coneguts meus) és que permet fer [trucades als números gratuïts](http://www.freeworlddialup.com/advanced/toll_free_connection) (_toll-free_) d'Estats Units, Paios Baixos, Regne Unit, Noruega, Alemanya i Japó, sense pagar un ~~duro~~ euro :)

<!--more-->  
La configuració finalment ha quedat d'aquesta manera:

Cal canviar `646499` pel vostre número d'usuari, `200` per la extensió local a la que voleu redirigir la trucada quan algú us truqui al número de FWD, i `pauoliva` pel vostre username d'IAXTel.

`iax.conf`

```
[general] register =\> pauoliva:xxx@iaxtel.com register =\> 646499:xxxx@iax2.fwdnet.net ; Trust Caller\*ID Coming from iaxtel.com [iaxtel] type=user context=default auth=rsa inkeys=iaxtel ; Trust Caller\*ID Coming from iax.fwdnet.net [iaxfwd] type=user context=default auth=rsa inkeys=freeworlddialup [fwd-gw] ; outbound connections to FWD type=peer auth=md5 secret=xxxx username=646499 qualify=yes host=iax2.fwdnet.net disallow=all allow=ulaw callerid="Pau"\<646499\> [iaxtel-gw] ; outbound connections to iaxtel type=peer auth=md5 secret=xxx username=pauoliva qualify=yes host=iaxtel.com disallow=all allow=ulaw callerid="Pau"\<17007931122\>
```

`extensions.conf`

```
[iaxfwd] exten =\> \_001.,1,SetCallerId,646499 exten =\> \_001.,2,Dial(IAX2/646499@fwd-gw/${EXTEN:3},60,r) exten =\> \_001.,3,Congestion [fromiaxfwd] exten =\> 646499,1,Dial(646499,20,r) exten =\> 646499,2,Voicemail,u200 exten =\> 646499,102,Voicemail,b200 [iaxtel] exten =\> \_1700NXXXXXX,1,Dial(IAX2/pauoliva@iaxtel-gw/${EXTEN}@iaxtel) exten =\> \_1888NXXXXXX,1,Dial(IAX2/pauoliva@iaxtel-gw/${EXTEN}@iaxtel) exten =\> \_1877NXXXXXX,1,Dial(IAX2/pauoliva@iaxtel-gw/${EXTEN}@iaxtel) exten =\> \_1866NXXXXXX,1,Dial(IAX2/pauoliva@iaxtel-gw/${EXTEN}@iaxtel) exten =\> \_1800NXXXXXX,1,Dial(IAX2/pauoliva@iaxtel-gw/${EXTEN}@iaxtel)
```

I afegiu això al _context_ de les extensions que vulgueu que puguin utilitzar el servei:

```
; Permeto rebre trucades usant FWD include =\> fromiaxfwd ; Permeto trucar amb iaxtel include =\> iaxtel
```

Amb aquesta configuració, per trucar a un número gratuït d'estats units amb IAXTel, només caldrà marcar el número (1800..) sense cap prefix. Si volem trucar al mateix número però amb FWD marcarem el 001 al davant.  
Per fer proves de trucada sortint us recomano utilitzar el (001)613 que és el _echo-test_ de FWD, on també podrem accedir des-de IAXTel si marquem el 17009999613. Aquí teniu més [números de FWD](http://www.freeworlddialup.com/advanced/service_numbers). Per provar la trucada entrant utilitzeu el servei [Call Me](http://account.freeworlddialup.com/index_new.php?section_id=76), si tot està ben configurat rebreu una trucada i entrareu en una _conference call_.

