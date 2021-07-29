---
layout: post
title: Configuració de la tarjeta X100P amb asterisk
date: 2005-05-08 03:05:48.000000000 +02:00
type: post
categories:
- gadgets
- linux
- voip
tags: []
meta:
  enclosure: |-
    /blog/wp-content/x100p.jpg
    5294
    image/jpeg
  tags: ''
permalink: "/2005/05/08/configuracio-de-la-tarjeta-x100p-amb-asterisk/"
---
Ja tinc aquí la [X100P](/blog/2005/04/21/x100p-oem-fxo-pci-card/) que vaig demanar fa uns dies. Després de punxar-la al server i perdre el uptime, el procés de configuració és bastant senzill. Instal·lem el paquet `zaptel`, que compilarà els mòduls per a les targetes Digium i ZapataTelephony. Si utilitzem kernel 2.6 haurem de tenir el sistema amb _[udev](http://www.kernel.org/pub/linux/utils/kernel/hotplug/udev.html)_, ja que zaptel no suporta _devfs_.

![x100p]({{ site.baseurl }}/assets/images/2005/05/x100p.png)

<!--more-->

Un cop els mòduls estan compilats, carreguem el mòdul wcfxo, corresponent a la targeta x100p: `modprobe wcfxo`

Al carregar els mòduls _zaptel_ y _wcfxo_ m'he trobat amb aquest error:

```
zaptel: Unknown symbol crc\_ccitt\_table wcfxo: Unknown symbol zt\_receive wcfxo: Unknown symbol zt\_ec\_chunk wcfxo: Unknown symbol zt\_transmit wcfxo: Unknown symbol zt\_unregister wcfxo: Unknown symbol zt\_hooksig wcfxo: Unknown symbol zt\_register wcfxo: Unknown symbol zt\_alarm\_notify
```

He trobat la sol·lució [aquí](http://lists.digium.com/pipermail/asterisk-users/2004-October/066542.html), es tractava només de compilar el kernel amb `CONFIG _ CRC _ CCITT=m`, llavors al fer el `modprobe wcfxo` ha detectat correctament la targeta:

```
Zapata Telephony Interface Registered on major 196 ACPI: PCI interrupt 0000:00:06.0[A] -\> GSI 17 (level, low) -\> IRQ 17 wcfxo: DAA mode is 'FCC' Found a Wildcard FXO: Generic Clone
```

Després he hagut de recompilar el asterisk amb suport de zaptel, per tenir el mòdul `chan_zap.so` dins de `/usr/lib/asterisk/modules`, ja que quan vaig instal·lar asterisk no li vaig donar suport.

Configuració de `/etc/zaptel.conf`:

```
# define spain tone zone loadzone = es defaultzone=es # Use Kewlstart FXS signalling for the Wildcard X100P fxsks=1
```

Veiem que vol dir el paràmetre `fxsks=1`: (font [voip-info](http://www.voip-info.org/wiki-Asterisk+config+zaptel.conf))

> fxsks=1: is actually 3 parameters specified in one chunk. Let's break the chunk down first, to make the parts easier to understand. The fist part in this chunk is "fxs", which means use FXS signalling for this interface card so it can communicate with the public telephone network. The next part in this chunk is "ks", which means use kewlstart signalling to determine whether a channel is open or closed (analogous to a telephone handset being off-hook or on-hood, respectively). The next part in this chunk is "=1" which means this interface card will be identified as channel 1 in other configuration files; especially in extensions.conf (also known as the "Dialplan"). For example, this card will be referenced as Zap/1 in extensions.conf.

Finalment, configurem asterisk per a que reconegui la nova interface Zap/1, corresponent a la línia telefònica.

Configuració de `/etc/asterisk/zapata.conf`:

```
signalling=fxs\_ks language=en context=usuaris ;callprogress=yes ; try to detect disconnect supervision busydetect=yes ; try to detect disconnect supervision busycount=6 immediate=no channel =\> 1
```

Amb la x100p m'he trobat també amb el problema de que no detecta el _hangup_ per que el meu proveïdor de línia telefònica (auna) no ofereix [disconnect supervision](http://www.voip-info.org/wiki-Asterisk+Disconnect+Supervision), un senyal de 48 volts de notificació de fi de trucada. He intentat sol·lucionar-ho jugant amb els paràmetres `callprogress, busydetect i busycount` però sembla que encara no acaba de funcionar del tot bé. Una sol·lució que encara tinc que provar és el [Absolute timeout](http://www.voip-info.org/wiki-Asterisk%20AbsoluteTimeout), per a establir el temps màxim de trucada.

Un cop hem arribat aquí, configurarem les extensions al arxiu `extensions.conf`, primer una extensió per a fer trucades a través de la línia telefònica:

```
[menta] exten =\> \_155.,1,Dial(Zap/1/${EXTEN:3},60,r) exten =\> \_155.,2,Hangup
```

D'aquesta manera quan vulguem trucar a un fix nacional per exemple, podem marcar 155 i el número de telèfon fix per a fer la trucada amb la línia telefònica fixa. Evidentment haurem de donar permís a les extensions que vulguem que puguin utilitzar el context _menta_ que acabem de crear. Per exemple si volem que els usuaris del context pofhq pugin utilitzar la línia de telèfon afegirem al final del context pofhq la següent línia:

```
include =\> menta
```

A continuació, farem que quan algú truqui al nostre número de telèfon fix, si no despenjem el telèfon prèviament li atengui la trucada asterisk. Si el callerid no és conegut, només permetem que puguin trucar a les extensions internes (o deixar-los un missatge al contestador si estan ocupades o no hi son). En canvi, si el callerID és el meu número de mòbil haig de poder fer trucades amb [voipjet](/blog/2005/04/21/asterisk-voipjet/). Com el callerID és molt fàcil de spoofejar també posarem protecció sol·licitant un [password](http://www.voip-info.org/wiki-Asterisk+user+authentication) amb <acronym title="Direct Inward System Access"><a href="http://www.voip-info.org/wiki-Asterisk+cmd+DISA">DISA</a></acronym>. La configuració per fer això al context per defecte de `extensions.conf` es la següent:

```
; esperem 1 segon abans de contestar exten =\> s,1,Wait,1 ; Wait 1 seconds ; si el callerID és 666666666 exten =\> s/666666666,2,Answer ; contesta exten =\> s/666666666,3,DigitTimeout(5) ; set digit timeout 5 sec exten =\> s/666666666,4,ResponseTimeout(10) ; response timeout 10 sec exten =\> s/666666666,5,Authenticate(1234) ; password (canviar-lo pel que es vulgui usar) exten =\> s/666666666,6,DISA(no-password|pofhq) ; si el password es correcte transfereix al contexte ; si el callerid es qualsevol altre, enviem al context usuaris que només permet fer trucades a extensions internes exten =\> #,1,Answer ; contesta exten =\> #,2,DigitTimeout(5) ; set digit timeout 5 sec exten =\> #,3,ResponseTimeout(10) ; response timeout 10 sec exten =\> #,4,Playback(pof-calabria) ; Let them know what's going on exten =\> #,5,DISA(no-password|usuaris) ; transfereix al contexte sense demanar paswd ; després del timeout quan no coincideix el callerid, enviem a #,1 exten =\> t,1,Goto(#,1) exten =\> i,1,Playback(invalid) ; "That's not valid, try again"
```

Si us fixeu, després de contestar la trucada abans de donar to, faig un Playback del fitxer _pof-calabria_, aquest fitxer és un fitxer gravat en format GSM que dona la benvinguda i informa a la persona que truca de quins números d'extensió pot marcar. Per crear aquest fitxer, gravem un wav i el convertim a gsm amb [sox](http://sox.sourceforge.net/), i després el copiem a `/var/lib/asterisk/sounds/`:

```
# sox inputfile.wav -r 8000 -c 1 outputfile.gsm resample -ql # cp outputfile.gsm /var/lib/asterisk/sounds/
```

I això és tot... ara puc trucar a estats units des-de el mòbil trucant-me al telèfon fixe de casa, i fer sortir la trucada a través d'internet amb voipjet... pagant la tarifa del mòbil al fixe + 1 cèntim el minut de voipjet, o deixar-li un missatge al meu company de pis a la bústia de veu del asterisk trucant al fixe de casa, i que rebi el missatge al instant per correu electrònic. Interessant, no? :)

