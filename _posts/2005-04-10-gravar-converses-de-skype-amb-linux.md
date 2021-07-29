---
layout: post
title: Gravar converses de skype amb linux
date: 2005-04-10 21:11:19.000000000 +02:00
type: post
categories:
- personal
- voip
tags: []
meta:
permalink: "/2005/04/10/gravar-converses-de-skype-amb-linux/"
---
Fa un temps [Marilen Corciovei](http://len.is-a-geek.org/) va penjar [aquest post](http://forum.skype.com/viewtopic.php?p=87378) on publicava una versió modificada de [vsound](http://www.zorg.org/vsound/) que permet gravar converses de Skype. Ahir em vaig decidir a provar-lo y vaig veure que algunes coses fallaven, molts cops els interolocutors de la conversa anaven desfassats l'un de l'altre i de vegades ni tan sols es gravava. Després de mirar una mica el codi vaig veure on estava el problema i he implementat un mecanisme molt bàsic de control que sol·luciona el problema de l'audio desfasat i sempre grava correctament la última conversa que hem realitzat. Si us el voleu descarregar el teniu aquí: [skype-rec.tar.gz](/projects/skype-rec.tar.gz).

Per a executar-lo només cal fer `./skype-rec /opt/skype/skype` substituint el path per el lloc on tingueu instal·lat l'executable de skype. La última conversa queda gravada en un fitxer quan es tanca Skype.

Skype-rec funciona de la mateixa manera que vsound, es carrega en forma de lliberería dinàmica amb LD\_PRELOAD i substitueix algunes crides bàsiques al sistema com open() read() i write() per guardar la entrada de micròfon i la sortida d'altaveus en dos fitxers separats. Després aquest dos fitxers es barregen amb sox, per tant per a que funcioni també heu de tenir [sox](http://sox.sourceforge.net/) instal·lat.

Espero que us sigui útil! :)

**UPDATE** : [Nathan Poznick](http://wang-fu.org/) (kraken), ha modificat la meva versió per a que grave totes les converses i ha reescrit el script per a llançar skype en perl, afegint-li les modificacions pertinents per a que barregi totes les converses que s'han gravat. Podeu seguir la història [en el thread original](http://forum.skype.com/viewtopic.php?p=87378) als forums de Skype. La versió de kraken es pot descarregar [aqui](http://z0g.org/stuff/skype-rec-kraken.tar.gz).

