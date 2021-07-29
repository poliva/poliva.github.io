---
layout: post
title: Gentoo tips
date: 2004-11-02 03:00:39.000000000 +01:00
type: post
categories:
- linux
tags: []
meta:
permalink: "/2004/11/02/gentoo-tips/"
---
A les últimes _Gentoo Weekly Newsleters_ han anat surtint cosetes interessants que recopilo a continuació.

· [GWN October 18, 2004](http://www.gentoo.org/news/en/gwn/20041018-newsletter.xml):  
A la secció _tips and tricks_ explica com funcionen els initscripts de gentoo, a nivell d'usuari. La documentació mes complerta es pot trovar a [la guia de Initscripts de Gentoo](http://www.gentoo.org/doc/en/handbook/handbook-x86.xml?part=2&chap=5).

· [GWN October 25, 2004](http://www.gentoo.org/news/en/gwn/20041025-newsletter.xml):  
A la secció _Gentoo News parla_ de les noves funcions de Portage 2.0.51, jo ja vaig avançar algo [fa un temps](/blog/2004/09/29/79/), ara ja està disponible [l'anunci oficial](http://www.gentoo.org/news/20041021-portage51.xml). També a _tips and tricks_ parla de la opció `'--newuse'` que permet recompilar les aplicacions que fan us d'una _use flag_ determinada después de que aquesta hagi estat modificada, l'exemple que posa és aquest: si no tenim impresora, tindrem la _use flag_ `cups` deshabilitada, si ens comprem una impresora i habilitem USE="cups" només cal que fem un `emerge --newuse` y totes les aplicacions que teniem instalades que fan servir aquesta _use flag_ es recompilaran amb soport de cups.

· [GWN November 1, 2004](http://www.gentoo.org/news/en/gwn/20041101-newsletter.xml):  
Aquesta setmana ha hagut alguns _threads_ que parlen sobre com utilitzar correctament la variable USE: [USE flags documentation](http://thread.gmane.org/gmane.linux.gentoo.user/105145), [Choosing USE flags (and choosing well)](http://thread.gmane.org/gmane.linux.gentoo.user/105001) i [changed USE flags](http://thread.gmane.org/gmane.linux.gentoo.user/104703).  
Finalment, a la secció _Tips and tricks_ expliquen com utilitzar la variable `PORTAGE_NICENESS` del `make.conf` per a baixar la prioritat de portage, molt útil quan fem emerges amb el workstation amb el que estem treballant al moment de compilar.

