---
layout: post
title: Clients VoIP per a Linux
date: 2006-04-03 22:45:23.000000000 +02:00
type: post
categories:
- linux
- voip
tags: []
meta:
  tags: linux voip ekiga skype gizmo wengo zfone
permalink: "/2006/04/03/clients-voip-per-a-linux/"
---
Aquests últims dies m'ha donat per provar alguns serveis de VoIP en linux que feia temps que tenía pendents, aquí podeu veure una captura de pantalla on surten [Skype](http://www.skype.com/), [Ekiga](http://www.ekiga.org/), [WengoPhone](http://www.openwengo.org/) i [Gizmo](http://www.gizmoproject.com/).

![linux voip skype gizmo wengo ekiga]({{ site.baseurl }}/assets/images/2006/04/voip-clients-linux.jpg)

**[Ekiga](http://www.ekiga.org)** és Open Source i utilitza estàndars oberts, SIP per a veu i H.323 per a video. Permet fer trucades a telèfons fixes usant el servei de [Diamond](http://www.diamondcard.us/), les trucades a Espanya surten per 1,8 cent/min a fixes i 19 cent/min a movils, s'ha d'habilitar a través de la opció _PC-to-Phone settings_, les trucades entre usuaris de ekiga son gratuïtes, i a més permet afegir multiples accounts SIP i utilitzar-los simultàniament: tenim independència del proveïdor. No m'ha donat problemes per usar-lo darrera d'un NAT utilitzant un servidor STUN de ekiga, i connecta perfectament amb l'[asterisk](http://www.asterisk.org) de dins de la xarxa local de casa i s'integra amb els contactes del Evolution :) A partir d'ara és el softphone que utilitzo a diari. Només està disponible per Linux.

**[Skype](http://www.skype.com)** utilitza un protocol propietari i no és de codi obert. Existeixen versions per a Linux, MacOS X, Windows i Windows Mobile (PocketPC). La versió de Windows és la mes avançada i soporta videoconferència, la resta encara no. La xarxa és tancada i no s'hi pot connectar amb altres clients, ni soporta connexió a altres xarxes ni altres protocols. Les [tarifes](http://www.skype.com/go/allrates) per trucar a telèfons fixes son una mica mes cares que amb la resta de proveïdors, però juguen amb que tenen molta quota de mercat. Les trucades a fixes d'Espanya surten a 2 cent/min i a mòvils a 25,3 cent/min. També permet adquirir un telèfon fix (DID) de diversos paisos, per 30€ al any. El servei de bustia de veu també és de pagament per 15€ al any. El protocol que utilitza no dona problemes si estàs darrera d'un NAT.

**[Gizmo](http://www.gizmoproject.com)** utilitza estàndars oberts (SIP i XMPP) però és de codi tancat, té una inteface gràfica molt maca, és molt fàcil d'utilitzar i permet fer trucades a altres telèfons SIP d'altres xarxes, a Gizmo i a GoogleTalk de forma gratuïta, també te un servei de [CallOUT](http://www.gizmoproject.com/call-out.html) per trucar a telèfons normals que surt a 1,9 cent/min a fixes d'Espanya i 28,2 cent/min a mòvils i un servei de [CallIN](http://www.gizmoproject.com/call-in.php) amb números de USA i UK per 28,9€ al any. El protocol que utilitza per veu és SIP, per tant ens poden trucar al account de Gizmo desde qualsevol altre softphone. Es diferècia de la resta perque permet gravar les converses només pulsant un botó mentre estem en una trucada. Està disponible per a Linux, MacOS X i Windows, i no suporta video en cap de les seves versions.

**[WengoPhone](http://www.openwengo.org/)** és Open Source i de moment només està disponible per Linux i Windows, i pròximament treuràn una versió per Windows Mobile (PocketPC) i per a MacOS X. També tenen una extensió per firefox que permet fer trucades seleccionant un número de telèfon desde el navegador. Suporta veu i video utilitzant estàndards oberts, trucades gratuïtes a altres usuaris de wengo i trucades a telèfons fixes d'Espanya a 1 cent/min i a mòvils d'Espanya per 18 cent/min. També permet enviar SMS. No se si és perque l'estic utilitzant desde IceWM però hi ha opcions que em fallen, sobre tot les coses que se suposa que han d'obrir un navegador com per exemple la búsqueda de contactes, així que no l'he pogut provar massa, pel que sembla s'integra millor amb KDE.

Aquí teniu els meus contactes, per si us interessa provar-ne algún:

- Ekiga: <tt>pauoliva@ekiga.net</tt>
- WengoPhone: <tt>pauoliva</tt>
- Gizmo: <tt>pauoliva</tt>
- Skype: <tt>pauoliva</tt>

Per cert, també he compilat el [Zfone](http://www.philzimmermann.com/EN/zfone/) de Philip Zimmermann que permet xifrar el tràfic de les converses SIP, seguint les [instruccions](http://www.kriptopolis.org/node/2045) de José Manuel a kriptopolis, però no tinc cap contacte per provar-lo, algú s'anima? ;)

