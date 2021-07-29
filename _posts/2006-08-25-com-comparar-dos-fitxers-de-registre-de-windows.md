---
layout: post
title: Com comparar dos fitxers de registre de windows
date: 2006-08-25 18:02:26.000000000 +02:00
type: post
categories: []
tags: []
meta:
  tags: wince pocketpc regedit registry windows diff cab regdelta
permalink: "/2006/08/25/com-comparar-dos-fitxers-de-registre-de-windows/"
---
Últimament estic flashejant una ROM nova a la PDA cada molt poc temps, i m'he cansat de anar customitzant el WM5 després de reinstalar cada cop, aixi que he exportat tot el registre després d'arrancar per primer cop i després he customitzat tot el que volia i he tornat a exportar el registre. El problema és que WM5 guarda el registre en format UTF16 i el <tt>diff</tt> només em deia que els fitxers eren diferents, però no me'ls comparava. He trobat una utilitat per comparar el registre que es diu [regdelta](http://smithii.com/?q=node/77), però és la versió 0.1 només funciona amb fitxers exportats per ell mateix, per tant no m'ha ajudat molt. Finalment he aconseguit fer el diff d'aquesta manera:

```
$ iconv -f utf16 -t iso-8859-15 full-reg1.reg \> full1.reg $ iconv -f utf16 -t iso-8859-15 full-reg2.reg \> full2.reg $ diff full1.reg full2.reg
```

Després he editat manualment el resultat del diff per deixar-lo correcte i m'he fet un .cab per instal·lar directament a la PocketPC amb [WinCE CAB Manager](http://www.ocpsoftware.com/products.php?nm=cecabmgr).

