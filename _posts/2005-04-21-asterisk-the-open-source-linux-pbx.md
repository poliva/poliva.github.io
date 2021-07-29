---
layout: post
title: 'Asterisk: The Open Source Linux PBX'
date: 2005-04-21 02:04:44.000000000 +02:00
type: post
categories:
- linux
- voip
tags: []
meta:
  enclosure: |-
    /blog/wp-content/asterisk.gif
    2252
    image/gif
  tags: ''
permalink: "/2005/04/21/asterisk-the-open-source-linux-pbx/"
---
![Asterisk]({{ site.baseurl }}/assets/images/2005/04/asterisk.png)

Un tema que feia temps que tenía pendent era l'[asterisk](http://www.asterisk.org), una <acronym title="Private Branch eXchange">PBX</acronym> per software que funciona amb Linux i proporciona totes les característiques que tenen les centraletes convencionals a més a més de veu sobre IP. Per a més infomació consultar [les _features_](http://www.asterisk.org/index.php?menu=features).

L'objectiu d'aquest post és mostrar la configuració minima d'una centraleta [Asterisk](http://www.asterisk.org/) per a poder fer una trucada interna entre dos usuaris. Com sempre els exemples son amb [Gentoo](http://www.gentoo.org) que és el que jo utilitzo, però es pot fer igualment amb qualsevol altre Linux.

Primer instalem Asterisk (`emerge asterisk` amb gentoo), un cop instal·lat ens crearà el directori `/etc/asterisk/` que conté tots els fitxers de configuració, tocarem els següents:

### sip.conf

Descomentem les linies:

```
allow=ulaw allow=ilbc
```

i a sota afegim la linia:

```
allow=gsm
```

Descomentem localnet, i posem la nostra adreça de xarxa:

```
localnet=192.168.1.0/255.255.255.0
```

Afegim els usuaris interns, en aquest exemple n'hi ha dos, Pau i Esteve que tenen les extensions 200 i 201 respectivament i formen part del context "pofhq". Podeu posar-li el nom que vulgueu, i recordeu canviar també el password:

```
[200] type=friend username=200 secret=posa\_aqui\_el\_password host=dynamic callerid="Pau" \<200\> mailbox=200 context=pofhq canreinvite=no reinvite=no transfer=yes callgroup=1 pickupgroup=1 nat=no [201] type=friend username=201 secret=posa\_aqui\_el\_password host=dynamic callerid="Esteve" \<201\> mailbox=201 context=pofhq canreinvite=no reinvite=no transfer=yes callgroup=1 pickupgroup=1 nat=no
```

### voicemail.conf

Sota la secció `[default]` afegirem les busties de veu dels usuaris, especificant també el seu e-mail per a que rebin el missatge en wave per correu:

```
200 =\> 200,Pau,pau@nospam.org 201 =\> 201,Esteve,esteve@nospam.org
```

### extensions.conf

Al final del fitxer afegim les següents linies, cal tenir en compte canviar el context _pofhq_ pel mateix que hem especificat anteriorment.

```
[pofhq] ;include =\> default include =\> sip include =\> voicemail include =\> local [voicemail] exten =\> 200,1,SetLanguage(es) exten =\> 200,2,VoicemailMain2() exten =\> 200,3,Hangup [sip] exten =\> 200,1,Dial(SIP/200,20,t) exten =\> 200,2,Voicemail(200) exten =\> 200,3,Hangup exten =\> 200,102,Voicemail(200) exten =\> 200,103,Hangup exten =\> 201,1,Dial(SIP/201,20,t) exten =\> 201,2,Voicemail(201) exten =\> 201,3,Hangup exten =\> 201,102,Voicemail(201) exten =\> 201,103,Hangup
```

Finalment arranquem el Asterisk, amb Gentoo podem fer un `/etc/init.d/asterisk start`, sino podem llançar-lo manualment per fer proves amb la comanda:

```
# asterisk -vvvc
```

Un cop el asterisk està arrancat al sistema, podem entrar a la consola amb la comanda:

```
# asterisk -vvvr
```

Si fem un '`help`' veurem les comandes disponibles, per exemple fent un `sip show peers` veurem els usuaris que hi ha connectats.

Seguidament ja podem arrancar el [kphone](http://www.wirlab.net/kphone/) o qualsevol altre telèfon SIP i fer trucades del 200 al 201 :)

