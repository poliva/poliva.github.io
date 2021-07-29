---
layout: post
title: 'Una petita joya de gentoo: g-cpan.pl'
date: 2004-11-08 03:45:54.000000000 +01:00
type: post
categories:
- linux
tags: []
meta:
permalink: "/2004/11/08/una-petita-joya-de-gentoo-g-cpanpl/"
---
Els usuaris de [Gentoo](http://www.gentoo.org) us haureu trobat alguna vegada amb el típic problema dels mòduls de perl que no estàn al portage, ho resumeixo per als que no saben de que va el tema: Quan volem utilitzar un script en perl que depèn d'algún mòdul que no està al portage, s'ha de recurrir a CPAN per a instal·lar-lo, això comporta que el mòdul no quedi registrat al sistema de paquets i per tant es fà tediós de mantenir (actualitzar, desinstal·lar...), i no cal dir el sarau que es monta quan el mòdul que volem instal·lar té dependències d'altres mòduls (que poden o no estar al portage).

Per fí [he trobat la sol·lució](http://thread.gmane.org/gmane.linux.gentoo.user/106090), es diu `g-cpan.pl` i és un petit script (inclòs al portage) que genera el ebuild per al mòdul de CPAN que necessitem i l'instal·la al sistema registrant-lo al sistema de paquets i portant el control de dependències automàtic (si estàn a portage les agafa del portage, si no crea el ebuild per a la dependència i la instal·la igualment).

