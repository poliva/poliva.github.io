---
layout: post
title: Pila genèrica de 802.11 per al kernel 2.6
date: 2004-06-22 16:21:00.000000000 +02:00
type: post
categories:
- linux
- wireless
tags: []
meta:
permalink: "/2004/06/22/pila-genrica-de-80211-per-al-kernel-26/"
---
Llegeixo via [KernelTrap](http://kerneltrap.org/node/view/3245) que Jeff Garzik, l'encarregat de mantenir els drivers de dispositius de xarxa al kernel de Linux va anunciar fa unes setmanes la creació de la branca de desenvolupament de wireless-2.6, que intenta unificar els esforços que han fet els desarrolladors dels diferents drivers per a wireless que hi ha disponibles en Linux per a crear una única pila genèrica de 802.11 i que s'inclourà en el kernel per a que la puguin utilitzar tots els drivers i no tenir fins a tres implementacions diferents com tenim fins ara. Degut a que el projecte més madur que hi ha en aquest camp es el driver [HostAP](http://hostap.epitest.fi/), s'ha escollit aquest com a punt de partida per a la nova pila genèrica.

Jouni Malinen, el desarrollador de HostAP va publicar [aquest patch](http://hostap.epitest.fi/hostap-linux.tgz) per a 2.6.6 que bàsicament és el codi de hostap sense el codi necessari per a mantenir la compatibilitat amb 2.4, els mòduls pcmcia-cs i les diferents versions de wireless-extensions. A més, la part de xifrat implementada en hostAP (ARC4, Michael MIC y AES) s'ha substituït per a que utilitzi la crypto API que ja inclou el kernel 2.6.

Finalment Jeff Garzik està mantenint la branca [wireless-2.6](http://www.kernel.org/pub/linux/kernel/people/jgarzik/patchkits/2.6/) sincronitzada amb el codi de CVS de hostap i que de moment inclou els drivers hostap i el relativament nou driver de [Intel Centrino](http://ipw2100.sourceforge.net/) compartint la pila 802.11.

Pel que sembla aquest codi encara es molt experimental i no s'inclourà en el kernel fins que no s'hagi pulit una mica més i s'hagin extret completament les parts del codi que denoten que es un _merge_ d'un driver separat del nucli.

