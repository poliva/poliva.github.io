---
layout: post
title: Familiar Linux from scratch  per a HTC BlueAngel
date: 2006-01-13 04:37:07.000000000 +01:00
type: post
categories:
- HTC
- linux
tags: []
meta:
  tags: HTC blueangel linux familiar GPE opie monotone openembedded bitbake handhelds
permalink: "/2006/01/13/construir-imatge-linux-per-a-blueangel/"
---
![blueangel linux]({{ site.baseurl }}/assets/images/2006/01/xdaiii.png)

En el [aquest post](/blog/2006/01/07/linux-en-htc-blueangel/) explicava com arrancar una imatge de Familiar Linux sobre dispositius HTC BlueAngel, la imatge concretament és una imatge de [GPE](http://gpe.handhelds.org/) que hi ha a [gnulinux.biz](http://gnulinux.biz/), del 11 de Desembre de 2005. Gràcies a l'ajuda de [lkcl](http://lkcl.net/) al #htc-blueangel del IRC (irc.freenode.net) he aconseguit entendre com funciona tot aquest tinglado de [OpenEmbedded](http://www.openembedded.org) per construir una imatge desde cero.

OpenEmbedded és un entorn de desenvolupament que permet cross-compilar un Linux (bootloader, kernel i aplicacions) a mida per a una gran varietat de dispositius sobre diferents arquitectures de hardware (ipaq's, zaurus, wrt54g, nslu2, pvr's...).

Les instruccions per a crear l'entorn de desenvolupament sobre la nostra màquina estàn a [la pàgina GettingStarted](http://oe.handhelds.org/cgi-bin/moin.cgi/GettingStarted) de [OE](http://www.openembedded.org), i és amb això amb el que m'he basat per a fer aquest post.

El <acronym title="Software Configuration Management">SCM</acronym> que utilitza OpenEmbedded és [monotone](http://venge.net/monotone/), és fàcil acostumar-te a usar-lo si has tocat abans algún sistema de control de versions com CVS o Subversion.

La utilitat per a la gestió de paquets de OpenEmbedded és [BitBake](http://developer.berlios.de/projects/bitbake/), funciona de manera similar a <tt>emerge</tt> ja que és una utilitat derivada del portage de Gentoo. Aquí podeu veure el [manual de BitBake](http://bitbake.berlios.de/manual/).

Un cop feta aquesta petita introducció, anem a veure com preparar un sistema Gentoo GNU/Linux amb OpenEmbedded per poder crear la nostra imatge per a HTC Blueangel. Els passos son vàlids per a qualsevol altra distribució cambiant <tt>emerge</tt> per les comandes adeqüades, i per a qualsevol altre dispositiu que utilitzi OpenEmbedded, no ha de ser necessariament una HTC BlueAngel, sino que podría ser perfectament un Linksys wrt54g.

<!--more-->

Comencem instal·lant BitBake, monotone i psyco (un mòdul de python que accelera la execució del codi):

```
# emerge bitbake monotone psyco
```

Ara creem la estructura de directoris necessaria, jo he utilitzat com a base `/stuff`:

```
# mkdir /stuff
```

Tot el procés s'ha de poder fer com a usuari, així que és millor no treballar mai amb BitBake com a root perqué podem fer malbé el sistema, per tant canviem els permisos del `/stuff` per a l'usuari que utilitzarem per generar el nostre entorn de compilació.

```
# chown pau:users /stuff # su - pau $ cd /stuff $ mkdir build
```

Seguidament baixem un _daily snapshot_ de la base de dades de OpenEmbedded, això ens estalviarà molt de temps, ja que tot el repositori ocupa bastant i el monotone és molt lent:

```
$ wget [http://ewi546.ewi.utwente.nl/OE/OE.db.bz2](http://ewi546.ewi.utwente.nl/OE/OE.db.bz2)$ bzip2 -d OE.db.bz2 $ mv OE.db oe.db
```

Sincronitzem la bbdd local amb l'arbre existent al servidor per als 3 repositoris, el <tt>oz354fam083</tt> conté la branca de desenvolupament de OpenZaurus 3.5.4 i familiar 0.83.

```
$ monotone --db=/stuff/oe.db pull ewi546.ewi.utwente.nl org.openembedded.{dreambox,dev,oz354fam083}
```

Creem la estructura de directoris local per als tres repositoris:

```
$ monotone --db=/stuff/oe.db checkout --branch=org.openembedded.{dreambox,dev,oz354fam083}
```

Creem la configuració local:

```
$ mkdir -p /stuff/build/conf $ cd /stuff/build $ wget [http://hands.com/~lkcl/blueangel/local.conf](http://hands.com/~lkcl/blueangel/local.conf)(canviar /home/lkcl/ per /stuff)
```

Per alguna raó misteriosa el bitbake de Gentoo peta, finalment jo he utilitzat la versió de bitbake de CVS:

```
# cd /stuff/ # svn co svn://svn.berlios.de/bitbake/trunk/bitbake
```

Així que per executar bitbake, primer cal fer això:

```
export PATH="/stuff/bitbake/bin:$PATH:/usr/local/arm/3.4.1/bin" export BBPATH="/stuff/build:/stuff/org.openembedded.dev"
```

Al fitxer `/stuff/org.openembedded.dev/conf/machine/blueangel.conf` s'ha d'afegir aixó, sino intenta compilar un kernel que no toca:

```
PREFERRED\_PROVIDER\_kernel = "xanadux-ba-2.6"
```

Per mantenir els repositoris al dia, periòdicament haurem d'anar executant aquestes comandes:

```
monotone --db=/stuff/oe.db pull ewi546.ewi.utwente.nl org.openembedded.dev monotone --db=/stuff/oe.db pull ewi546.ewi.utwente.nl org.openembedded.dreambox monotone --db=/stuff/oe.db pull ewi546.ewi.utwente.nl org.openembedded.oz354fam083 cd /stuff/org.openembedded.dev && monotone update cd /stuff/org.openembedded.dreambox && monotone update cd /stuff/org.openembedded.oz354fam083 && monotone update
```

Finalment ja tenim l'entorn preparat per construir la imatge per a BlueAngel. Si volem una imatge de GPE farem:

```
# bitbake gpe-image
```

Si volem una imatge de OPIE:

```
# bitbake opie-image
```

Si volem cross-compilar una sóla aplicació (generar l'ipk), per exemple del gomunicator:

```
# bitbake gomunicator
```

Algunes cosetes a tenir en compte:

- La compilació de tot l'entorn i les imatges tardarà unes quantes hores i necessita uns 5Gb d'espai a disc.
- La imatge resultant estarà al directori `tmp/deploy/images`.
- Els paquets ipk individuals per a cada aplicació estaràn al directori `tmp/deploy/ipk`.
- El crosscompilador gcc estarà al directori `tmp/cross/bin`.

Espero que us sigui util! :)

