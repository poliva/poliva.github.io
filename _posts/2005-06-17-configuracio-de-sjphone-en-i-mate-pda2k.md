---
layout: post
title: Configuració de SJPhone en I-mate PDA2k
date: 2005-06-17 23:16:52.000000000 +02:00
type: post
categories:
- HTC
- voip
tags: []
meta:
  tags: telephony voip pda pocketpc sjphone
permalink: "/2005/06/17/configuracio-de-sjphone-en-i-mate-pda2k/"
---
Quan vaig parlar del [software per a PocketPC que utilitzo](/blog/2005/05/22/nova-versio-de-la-rom-de-la-pda2k/) en la PDA2k vaig comentar que ni el [SJphone](http://www.sjlabs.com/) ni el [X-PRO](http://www.xten.com/index.php?menu=products&smenu=xproppc) funcionaven massa bé. Bàsicament tu pots sentir perfectament a l'altra persona, pero el que et sent a tú et sent tallat i amb molt de soroll, a part dels ecos i aquestes coses.

Avui el [Carles](http://santo-s.blogspot.com/) m'ha preguntat sobre aquest tema i al final m'he posat a _indagar_ a veure si trovaba alguna sol·lució per aquest problema, i [aquest post](http://forum.labs.softjoys.com/viewtopic.php?t=409) dels forums de SJLabs m'ha fet veure la llum ;)

Per millorar la qualitat hem d'anar a "Menu --\> Options --\> Audio --\> Advanced Settings" i jugar una mica amb els diferents valors. Després de fer varies proves, la configuració ideal per mi és la següent:

`
Driver buffer size, msec: 100
Driver input queue length: 10
Driver output queue length: 10
RTP jitter queue length: 32
`

També he de tenir activada la casella `Do Not Send Silence`.  
A part d'això, el codec amb el que millor qualitat obtinc és el `Microsoft CCITT G.711 A-Law CODEC`, i tinc marcades les opcions `Automatically adjust microphone volume` i `Automatically adjust silence detection level`.

Finalment, a la configuració d'audio de la PDA (Settings --\> System --\> Microphone AGC) ho he de tenir amb `Enable`.

Per fer la prova final he trucat al Carles amb [voipjet](http://www.voipjet.com) a un mòbil de UK, primer desde el [kphone](http://www.wirlab.net/kphone/) amb el PC i després desde el SJphone amb la PDA configurada tal com he descrit. La primera trucada es sentia perfecta i sense cap retard, la segona es sentia mes o menys bé i amb una mica de retard, però es podia mantenir una conversa perfectament, sense problemes a les dues bandes :D

