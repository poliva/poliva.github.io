---
layout: post
title: Actualització de la ROM de la PDA2k
date: 2004-12-30 02:45:50.000000000 +01:00
type: post
categories:
- gadgets
- HTC
tags: []
meta:
permalink: "/2004/12/30/actualitzacio-de-la-rom-de-la-pda2k/"
---
Per fí I-mate ha publicat la esperada [nova versió de ROM de la PDA2k](http://www.msmobiles.com/news.php/3446.html), no hi ha _release notes_, només aquest text:

> This ROM build provides some significant improvements in radio, audio, bluetooth and operating functionality. We appreciate your patience and understanding.

Per les poques proves que he fet amb la nova rom (fa menys d'una hora que la he posat) els problemes de connexió amb el Jabra BT250v s'han sol·lucionat, al menys ara em conecta a la primera i no triga tant en trovar-lo, però la senyal de bluetooth segueix fluctuant.

Aquest és el procés que jo he seguit per fer l'actualització:

1) Activesync  
2) Settings -- System -- Permanent save  
3) Programs -- xbackup (80 Mb - 50 min)  
4) Executar el [upgrade](http://www.clubimate.com/index.asp?PageAction=DEVICE_PDA2K_BT3) (15 minuts)  
5) Hard reset: Mantenir pulsat el botó de power al mateix moment que es fa un soft-reset  
6) Programs -- xbackup -- restore (25 min)

Abans:  
`
ROM version: 1.12.23 WWE
ROM date: 09/27/04
Radio version: 1.02.00
Protocol version 1337.32
ExtROM version: 1.12.169 WWE
`

Després:  
`
ROM version: 1.22.00 WWE
ROM date: 10/22/04
Radio version: 1.06.00
Protocol version 1337.38
ExtROM version: 1.12.169 WWE
ExtROM version: 1.22.162 WWE
`

M'he quedat sorprés perquè no he perdut cap dada, ni cap paràmetre de configuració, ni cap programa dels que tenia instal·lat... ~~tot funciona correctament~~ , sembla mentida que porti el sistema operatiu que porta ;)

**Update** : s'ha de tenir la [base connectada a l'alimentació](http://pocketpcdubai.infopop.cc/eve/ubb.x?q=Y&a=tpc&s=553609464&f=611104143&m=191101605&p=1) per fer el upgrade.

**Update 2** : No es recomanable utilitzar el xbackup tal com vaig fer jo quan fem un ROM upgrade, per que guarda fitxers del sistema operatiu i després els machaca. La solució més ràpida és utilitzar [sprite backup](http://www.spritesoftware.com) i seguir [aquests passos](http://spritesoftware.crmdesk.com/answer.aspx?id=66).

