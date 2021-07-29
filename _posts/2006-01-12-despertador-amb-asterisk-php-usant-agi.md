---
layout: post
title: Despertador amb Asterisk + PHP usant AGI
date: 2006-01-12 03:26:45.000000000 +01:00
type: post
categories:
- linux
- voip
tags: []
meta:
  tags: asterkisk wake-up call php AGI
permalink: "/2006/01/12/despertador-amb-asterisk-php-usant-agi/"
---
Rebuscant pel wiki de [voip-info.org](http://www.voip-info.org) vaig trobar [aquest despertador](http://www.voip-info.org/wiki/view/Asterisk+tips+Wake-Up+Call+PHP+with+Snooze%252FAnnoy) fet amb PHP utilitzant <acronym title="Asterisk Gateway Interface">AGI</acronym>. Com veureu és ideal per als _pájaros de noche_ ja que ens asegura que al dia següen ens despertarem a l'hora que toca.

El <acronym title="Asterisk Gateway Interface">AGI</acronym> permet utilitzar qualsevo llenguatge per programar comandes d'asterisk, podeu veure molts [exemples aquí](http://www.voip-info.org/wiki-Asterisk+AGI).

Per instal·lar el despertador només cal descomprimir el contingut del zip dins de `/var/lib/asterisk/agi-bin/` i afegir una entrada al dialplan (<tt>extensions.conf</tt>) que cridi al script extern `wakeup.php`. Jo ho he fet així:

```
; Wake-Up Call PHP with Snooze/Annoy Exten =\> 8502,1,agi,wakeup.php Exten =\> 8502,2,Hangup
```

D'aquesta manera, quan truquem al 8502, podrem activar el despertador a l'hora que vulguem. L'asterisk ens trucarà a l'hora desitjada, informant-nos de que podem:

- Pulsar "1" per a dormir 5 minuts més, l'asterisk ens tornarà a trucar al cap de 5 minuts.
- Pulsar "2" per a dormir 10 minuts més, l'asterisk ens tornarà a trucar al cap de 10 minuts.
- Pulsar "3" per a dormir 15 minuts més, l'asterisk ens tornarà a trucar al cap de 15 minuts.
- Pulsar "4" per despertar-nos.

Si pulsem "4", el script calcula 2 números aleatoris de dos xifres i ens els fa sumar, per assegurar-se de que realment ens hem despertat i tenim la ment despejada.. si ens equivoquem ens torna a trucar i no ens deixa adormir-nos!

