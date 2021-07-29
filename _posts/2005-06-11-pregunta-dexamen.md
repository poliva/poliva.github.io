---
layout: post
title: Pregunta d'examen
date: 2005-06-11 02:13:07.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
permalink: "/2005/06/11/pregunta-dexamen/"
---
L'any passat la [laia](/~laia/) em va fer una pregunta d'examen d'una assignatura de Sistemes Operatius on estudiaven algunes comandes bàsiques de UNIX. La pregunta em va deixar sorprés i em va ensenyar una cosa que no sabia, avui ho he recordat i aquí us ho deixo, com a curiositat...

**En quin dia de la setmana va caure el 7 de Setembre de 1752?**

Si vols saber la solució, llegeix el post complert...<!--more-->

La comanda `cal` ens mostra un calendari per pantalla, podem passar-li alguns paràmetres com per exemple l'any o el mes que volem veure, així per veure el Setembre del 1752 fariem `cal -m 9 1752`, aquesta és la sortida per pantalla:

```
$ cal -m 9 1752 September 1752 Mo Tu We Th Fr Sa Su 1 2 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30
```

Qué ha passat amb els dies del 3 al 13 de Setembre? La solució és fàcil: `man cal`

> The Gregorian Reformation is assumed to have occurred in 1752 on the 3rd of September. By this time, most countries had recognized the reformation (although a few did not recognize it until the early 1900's.) Ten days following that date were eliminated by the reformation, so the calendar for that month is a bit unusual.

L'any 1752 va ser l'any que Estats Units va adoptar el [calendari gregorià](http://ca.wikipedia.org/wiki/Calendari_gregorià) (Espanya el va adoptar al 1582) i es van haver d'eliminar 10 dies per compensar l'avançament  
de la primavera provocat perquè el calendari julià que s'utilitzava fins llavors no compensava la [precessió dels equinoccis](http://ca.wikipedia.org/wiki/Precessió_dels_equinoccis).

Si més no, és curiòs trovar-se una pregunta com aquesta en un examen de UNIX, et deixa ben descol·locat :)

