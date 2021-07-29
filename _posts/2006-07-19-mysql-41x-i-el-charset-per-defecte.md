---
layout: post
title: Mysql 4.1.x i el charset per defecte
date: 2006-07-19 15:11:05.000000000 +02:00
type: post
categories:
- linux
- pofHQ
tags: []
meta:
  tags: mysql charset encoding utf8 latin1
permalink: "/2006/07/19/mysql-41x-i-el-charset-per-defecte/"
---
He actualitzat la [mysql](http://www.mysql.com/) de 4.1.14 a 4.1.20 i el _charset_ per defecte ha canviat a `utf8` sense que me'n adonés... totes les bases de dades estaven en `latin1` i no es mostrava correctament el contingut del blog, ni d'aquest blog ni de tots els que tinc allotjats al servidor: [quetzal](pof.eslack.org/~quetzal/), [laia](/~laia/), [sergi & eva](/~sergi/), [dan](http://boston.eslack.org/)...

Quan me n'he adonat (gràcies Dan!) he recompilat mysql amb `USE="latin1"` per a seguir mantenint el charset `latin1` com a charset per defecte en la versió 4.1.

Aneu en compte si actualitzeu, que no us passi com a mí!

