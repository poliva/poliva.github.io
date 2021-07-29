---
layout: post
title: El galliner de  Sveasoft
date: 2004-07-22 17:22:25.000000000 +02:00
type: post
categories:
- gadgets
- linux
- wireless
tags: []
meta:
  tags: ''
permalink: "/2004/07/22/el-galliner-de-sveasoft/"
---
Avui llegeixo via slashdot una nova noticia sobre la suposada [violació de la GPL per part de Sveasoft](http://slashdot.org/articles/04/07/21/2255239.shtml?tid=193&tid=1). Per als que no estigueu enterats, [Sveasoft](http://www.sveasoft.com/) és una companyia que distribueix una de les modificacions més famoses del firmware del [Linksys WRT54G](http://www.linksys.com/products/product.asp?prid=508&scid=35), un access point que funciona amb Linux. Sveasoft va modificar el firmware original lliberat sota la llicencia GPL per Linksys, afegint-li una sèrie de millores i noves característiques que el fan molt més atractiu que el firmware original. Al principi, el firmware de Sveasoft es podia descarregar lliurement des del seu FTP, on publicaven dues versions, la estable que també contenia el codi font, i la inestable de la qual només feien públic el binari.

Degut a quantitat de crítiques i difamacions que no venen al cas, sveasoft va decidir no fer públiques les versions inestables, i cobrar $20 USD per una subscripció anual a qui les vulgues tenir. Aquí va començar el gran problema, la GPL obliga a distribuir el software tant en forma binària com en codi font i encara que es pugui vendre, permet la lliure redistribució, es a dir, que si jo em faig subscriptor, pago els 20 dòlars, i em descarrego una versió inestable del firmware, tinc dret a redistribuir-lo i a sol·licitar a Sveasoft el seu codi font. Sveasoft ha intentat que això no sigui possible, i per fer-ho ha imposat una restricció a la llicencia, on suposadament si tu decideixes redistribuir el seu firmware, tu ets el responsable a partir d'aquell moment, com si allò fos un _fork_ del seu producte i ells se'n renten les mans, a més a més, si ho fas et cancel·len la subscripció i ja no tens accés a descarregar-te més versions inestables del firmware.

Sumaritzant, aquesta "restricció" podríem dir que no et permet redistribuir el codi lliurement, tal com permet la GPL, concretament la part violada de la llicencia seria aquesta: _You may not impose any further restrictions on the recipients exercise of the rights granted herein._

També hi ha qui ho veu d'una altra manera: sveasoft no viola la llicencia GPL, sino que imposa unes restriccions per al suport (concretament el suport ofert a través dels fòrums des d'on es pot descarregar el firmware si has pagat i tens accés).

A mi personalment em sembla molt bé que una empresa que basa el seu model de negoci amb software lliure decideixi cobrar pel suport, el problema més greu que veig és la restricció imposada per Sveasoft en la redistribució del firmware per part de terceres persones i la seva forma d'actuar per aturar-ho, qualificada per alguns com "[prácticas mafiosas](http://blog.fluzo.org/index.php?p=106)".

