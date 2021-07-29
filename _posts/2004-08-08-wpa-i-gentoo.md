---
layout: post
title: WPA i Gentoo
date: 2004-08-08 03:47:13.000000000 +02:00
type: post
categories:
- linux
- wireless
tags: []
meta:
permalink: "/2004/08/08/wpa-i-gentoo/"
---
La [versió 0.2.4 de hostap es considera estable](http://hostap.epitest.fi/) desde el dia 17 de Juliol, però [encara no està inclosa al portage](http://bugs.gentoo.org/show_bug.cgi?id=57708) tot i que [Peter Johanson](http://dev.gentoo.org/~latexer/) ha començat a fer els ebuilds, esperem que no trigui molt perque aquesta versió de hostap és la única que te soport per a WPA.

Tampoc no hem tingut mai branca WPA de madwifi, per sort com he comentat en el post anterior [desde el 5 d'Agost la branca WPA s'ha unit amb el HEAD](/blog/2004/08/08/46/), aixi que per als impacients [aquí teniu el meu ebuild](http://bugs.gentoo.org/show_bug.cgi?id=59739) amb un snapshot de CVS d'avui (08 d'Agost) que funciona amb WPA.

Tampoc tenim [wpa\_supplicant](http://hostap.epitest.fi/wpa_supplicant/), encara que [Torbjörn Svensson s'ha currat un ebuild](http://bugs.gentoo.org/show_bug.cgi?id=52854#c7) que funciona bé, però que no té soport per madwifi, i [jo l'he modificat](http://bugs.gentoo.org/show_bug.cgi?id=52854#c9) per a que funcioni també amb madwifi.

A veure quan triguen tots aquests canvis en arribar al _portage oficial_ i per fí els usuaris de Gentoo podem gaudir de soport de WPA a la nostra distribució sense massa complicacions.

