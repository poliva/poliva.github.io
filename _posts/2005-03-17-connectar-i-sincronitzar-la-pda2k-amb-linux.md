---
layout: post
title: Connectar i sincronitzar la PDA2k amb Linux
date: 2005-03-17 03:37:40.000000000 +01:00
type: post
categories:
- gadgets
- HTC
- linux
tags: []
meta:
  enclosure: |-
    /blog/wp-content/thumb-evolution.jpg
    25354
    image/jpeg
  tags: ''
permalink: "/2005/03/17/connectar-i-sincronitzar-la-pda2k-amb-linux/"
---
A continuació descriuré el procés que he seguit per a connectar la i-mate PDA2k amb linux a través del cable USB, alguns passos son específics de <acronym title="la distribució que jo utilitzo"><a href="http://www.gentoo.org">Gentoo</a></acronym>, i d'altres específics de la PDA2k, però el procés es pot extrapolar a qualsevol altra distribució de linux i qualsevol altre dispositiu PocketPC.

El primer pas es configurar el kernel amb els mòduls necessaris, cal tenir suport de <acronym title="Point to Point Protocol">PPP</acronym>, USB\_SERIAL i USB\_SERIAL\_IPAQ inclosos en el kernel 2.6. Aquest últim mòdul no té suport per a la PDA2k, per tant haurem de modificar una mica el codi font per a que funcioni.

```
CONFIG\_PPP=m CONFIG\_PPP\_ASYNC=m CONFIG\_PPP\_SYNC\_TTY=m CONFIG\_PPP\_DEFLATE=m CONFIG\_PPP\_BSDCOMP=m CONFIG\_USB\_SERIAL=m CONFIG\_USB\_SERIAL\_IPAQ=m
```

Editem el fitxer `drivers/usb/serial/ipaq.h` i afegim el text marcat en negreta:

```
#define HTC\_VENDOR\_ID 0x0bb4 #define HTC\_PRODUCT\_ID 0x00ce #define HTC\_HIMALAYA\_ID 0x0a02 **#define HTC\_BLUEANGEL\_ID 0x0a05**
```

A continuació editem el fitxer `drivers/usb/serial/ipaq.c` i afegim:

```
{ USB\_DEVICE(HTC\_VENDOR\_ID, HTC\_PRODUCT\_ID) }, { USB\_DEVICE(HTC\_VENDOR\_ID, HTC\_HIMALAYA\_ID) }, **{ USB\_DEVICE(HTC\_VENDOR\_ID, HTC\_BLUEANGEL\_ID) },**
```

Tot seguit ja podem compilar els mòduls i fer el `make modules_install`, ja tindrem els mòduls necessaris per a que tot funcioni.

Quan connectem la PDA2k al USB veurem que es carrega el mòdul _ipaq_ i ens podem comunicar amb la PDA2k a través del dispositiu `ttyUSB0`.

```
usb 1-2: new full speed USB device using address 6 ipaq 1-2:1.0: PocketPC PDA converter detected usb 1-2: PocketPC PDA converter now attached to ttyUSB0
```

Ara instal·larem el software necessari per a poder fer tot el que volem:

```
# emerge synce synce-multisync\_plugin multisync synce-rra synce-kde
```

Seguidament, en una consola d'usuari, llançarem el `dccm`:

```
$ dccm
```

NOTA: si volem debugar utilitzarem les opcions `-d 3 -f`  
NOTA2: si tenim password a la PDA hem de llançar-lo amb la opció `-p PASSWORD`

A continuació establirem la connexió ActiveSync des-de una consola de root:

```
# synce-serial-config /dev/ttyUSB0 [local-ip-address:remote-ip\_address] # synce-serial-start
```

NOTA: el serial-config només cal fer-lo el primer cop.

Si tot ha anat bé veurem que s'ha creat la interface `ppp0` i que podem fer-li pings a la PDA2k i ja podrem utilitzar les [utilitats](http://synce.sourceforge.net/synce/tools.php) de [SynCE](http://synce.sourceforge.net/synce/) per línia de comandes, com per exemple `pstatus` que ens mostrara informació del sistema.

A continuació llançarem el `synce-trayicon` que ens posarà una icona al system tray des-de la que podrem llançar un nautilus per a navegar per l'arbre de directoris de la PDA, veure l'estat de càrrega de la bateria o instal·lar i desinstal·lar fitxers CAB amb el _software manager_. Podeu veure un exemple en la següent captura de pantalla (clic per ampliar):

[![SynCE]({{ site.baseurl }}/assets/images/2005/03/thumb-synce.png)](/archives/images/synce.png)

També hi ha el [equivalent per a KDE](http://synce.sourceforge.net/synce/kde/), per a llançar el _tray icon_ heu d'executar `raki` i segons tinc entès també deixa navegar pels fitxers des-de konqueror, però jo encara no l'he provat.

Seguidament configurarem el `multisync` per a sincronitzar els contactes, tasques i calendari amb Evolution 2. Executem `multisync` des d'una consola d'usuari i anem a `File --> New Sync Pair`, com a Primer plugin seleccionem _Ximian Evolution 2_ i com a segon plugin _SynCE Plugin_. Seleccionem _Calendar, AddressBook i Tasks_ o el que desitjem sincronitzar, acceptem i premem el botó _Sync_, veurem que automàticament l'Evolution i la PDA2k es sincronitzen, com en la següent captura de pantalla:

[![Evolution SynCE]({{ site.baseurl }}/assets/images/2005/03/thumb-evolution.png)](/archives/images/evolution.png)

I això és tot, espero que com a mínim a algú li sigui tant útil com a mi :D

