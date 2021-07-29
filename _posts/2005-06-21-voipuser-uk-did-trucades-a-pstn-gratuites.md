---
layout: post
title: 'voipuser: UK DID + trucades a PSTN gratuïtes'
date: 2005-06-21 20:30:45.000000000 +02:00
type: post
categories:
- voip
tags: []
meta:
  tags: voip telephony sip asterisk did
permalink: "/2005/06/21/voipuser-uk-did-trucades-a-pstn-gratuites/"
---
[VoIP User](http://www.voipuser.org/) és un proveïdor SIP que ofereix trucades gratuïtes a telèfons fixes [de més de 60 països](http://www.voipuser.org/forum_topic_336.html). Per aconseguir-ho et donen un <acronym title="Direct-Inward-Dial">DID</acronym> o numero de telèfon _"real"_ de la <acronym title="public switched telephone network">PSTN</acronym>, que seria l'equivalent a un 906, 803, 806, 807... aquí a espanya, es a dir, un número de tarificació especial on la persona que truca paga més diners dels que pagaria si fos un fixe normal. Amb els diners que es guanyen de totes les trucades fetes a aquests números es fa un pot global, i ens regalen els minuts de _sortida_ per fer trucades gratuïtes.

A la [pàgina principal de VoIP User](http://www.voipuser.org) podem veure un missatge que ens indica quants minuts hi ha disponibles per fer trucades a l'exterior, actualment és aquest: **There are 25314 minutes in the community pool**.

Quan utilitzem el servei, hem de fer-ho tenint amb compte la [política d'ús](http://www.voipuser.org/forum_topic_1783.html), que més o menys diu que podem fer un minut de trucada a la PSTN per cada minut entrant al nostre número 844 que fa incrementar el pot (ratio 1:1). El ratio canvia si en lloc de seleccionar un prefix 844 seleccionem un prefix 7031, ja que les trucades son molt mes cares i en aquest cas tenim un ratio aproximat 1:10 (10 minuts sortints per cada minut entrant). Podem configurar-nos els números de telèfon a través del [panel de control](http://www.voipuser.org/mynumbers.html) de VoIP User.

Per rebre trucades entrants, jo he optat per proporcionar el meu numero de UK (+447031901957) amb ratio 1:10 a tots els formularis de registre que empleno, així desvio cap a aquest número totes les trucades de telemarketing i demés _basura_ que rebo telefònicament, i redirigeixo les trucades al meu mòbil espanyol. També he activat el contestador, assegurant-me així de que totes les trucades que rebo es contestaran i generaran minuts encara que jo no tingui el mòbil enxufat.

Per als amics que tinc a UK tinc un segón número (+448449862541 des de l'extranger, o 08449862541 des de UK) on em poden trucar a preu de trucada local (0.05 £/min) i també generarán tràfic entrant a ratio 1:1.

Finalment m'he creat un tercer número (+448715047656) que utilitzo com a FAX, ja que VoIP User també proporciona una pasarela fax-to-email, així quan m'envien un fax a aquest número el rebo al cap d'uns minuts per correu electrònic.

S'ha de tenir en compte que si no es reb cap trucada al número proporcionat durant un periòde de 3 mesos ens poden cancelar el servei.

[En aquest post](http://www.voipuser.org/forum_topic_330.html) podeu veure com configurar l'asterisk per a utilitzar el servei de VoIP User.

