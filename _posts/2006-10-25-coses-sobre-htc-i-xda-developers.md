---
layout: post
title: Coses sobre HTC i xda-developers
date: 2006-10-25 07:24:31.000000000 +02:00
type: post
categories:
- gadgets
- linux
- personal
- trips
tags: []
meta:
  tags: htc hermes tytn trinity treo750 pocketpc simlock unlock rom bootloader linux
permalink: "/2006/10/25/coses-sobre-htc-i-xda-developers/"
---
![HTC Hermes]({{ site.baseurl }}/assets/images/2006/10/TyTN.png)

Fa dos mesos que no feia cap post... he estat mooolt liat aquests dos mesos, a part de [feina](http://www.kubiwireless.com) i [estudis](http://www.uoc.edu), desde que em vaig comprar la HTC Hermes (TyTN) m'he enganxat moltíssim a hackejar-la... de tant en tant rebo algun mail de gent que em pregunta coses sobre la Hermes, donçs totes les respostes les teniu a xda-developers:

- [Forums de HTC Hermes](http://forum.xda-developers.com/forumdisplay.php?f=254)
- [Wiki de HTC Hermes](http://wiki.xda-developers.com/index.php?pagename=HTC_Hermes)

Desde el primer moment el meu objectiu era bootar un kernel de Linux, per desgràcia això encara no és possible, pero havia de conèixer al màxim el dispositu per tal d'arribar a fer-ho. El primer pas va ser aprendre com funcionava el nou [format NBH](http://wiki.xda-developers.com/index.php?pagename=Hermes_NBH) dels upgrades de ROM, per poder extreure les diferents parts. Aquest format ha canviat respecte a versions anteriors de PDAs de HTC, pero pel que sembla tots els nous dispositius d'HTC utilitzaràn aquest format a partir d'ara (Treo750, Trinity, Artemis, Excalibur, StarTrk, etc...).

Despres he estat investigant moltissim sobre el [bootloader de la Hermes](http://wiki.xda-developers.com/index.php?pagename=Hermes_BootLoader), gràcies a l'ajuda de l'[esteve](http://esteve.tizos.net) vam descobrir com [autenticar-nos al bootloader](http://wiki.xda-developers.com/index.php?pagename=Hermes_BootLoaderPassword) amb el password correcte que es genera dinàmicament i permet utilitzar totes les comandes.

Gràcies a alguns contactes, he anat aconseguint ROMs en primicia, no només per a la Hermes sinó també per a altres dispositius d'HTC:

- HTC Hermes - [English Cingular 8525 ROM update available: v1.30 radio 1.11](http://forum.xda-developers.com/showthread.php?t=277664)
- HTC Hermes - [English Cingular 8525 ROM update (AKU2.6): v1.31 radio 1.13](http://forum.xda-developers.com/showthread.php?t=278001)
- HTC Hermes - [English Cingular 8525 ROM update available: v1.34 radio 1.16](http://forum.xda-developers.com/showthread.php?t=278386)
- HTC Cheetah - [New Cingular ROM upgrade for Palm Treo 750 (2006-10-21)](http://forum.xda-developers.com/showthread.php?t=280500)
- HTC Artemis - [New ROM version 1.11.405.1 WWE (2006-10-22)](http://forum.xda-developers.com/showthread.php?t=280647)

També he pogut [extreure roms de radio](http://wiki.xda-developers.com/index.php?pagename=Hermes_ExtractedRadioRoms), per actualitzar només la radio stack sense haver de fer hard reset, gràcies a les fotos que ha publicat la gent que ha obert la Hermes (jo encara no m'he atrevit!) hem anat descobrint [el hardware que porta dins](http://wiki.xda-developers.com/index.php?pagename=Hermes_HardwareOverview), molt útil per al [port de Linux](http://wiki.xda-developers.com/index.php?pagename=Hermes_Linux), he estat ajudant a gent que s'ha carregat la Hermes fent un upgrade, etc, etc... no acabaría mai: ja porto 870 posts als forums de xda-developers :)

Ara mateix estic intentant fer [enginyeria inversa sobre el unlocker de imei-check](http://forum.xda-developers.com/showthread.php?t=280819) per treure-li el simlock a la Hermes i oferir un unlocker gratuït, simplement per apendre com funciona la "security area" i la rom de radio :)

Ah! i se m'oblidava: em vaig comprar una segona HTC Hermes per ebay ($400), un model de preproducció amb _security level 0_ i SuperCID, molt útil per apendre com funciona la Hermes realment ;)

Ja us contaré més coses, ara vaig a preparar-me la maleta que demà me'n vaig cap a Alemania una setmaneta de vacances...

