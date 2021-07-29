---
layout: post
title: 'Gentoo tool: equery'
date: 2005-04-04 15:18:01.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
permalink: "/2005/04/04/gentoo-tool-equery/"
---
Per als usuaris de Gentoo que no conegueu la utilitat _equery_, inclosa dins de `gentoolkit` aquí va una petita explicació i exemples d'ús, que ben segur que us facilitarà alguna tasca dins de l'administració del sistema de paquets de Gentoo. Equery és la utilitat que reemplaça a _qpkg_ que si us heu fixat, en les últimes versions mostra un warning dient que només es corregeixen bugs però no s'afegeixen noves funcionalitats, fins que desaparegui en favor de equery.

> **`equery uses paquet`**  
> Ens mostra les variables use del paquet remarcant les que es van habilitar en temps de compilació per als paquets que tinguem instal·lats, amb la descripció de cada variable. (Similar al _etcat uses ..._)
> 
> **`equery belongs fitxer`**  
> Ens diu a quin paquet pertany el fitxer especificat. (Com el _qpkg -f ..._)
> 
> **`equery check paquet`**  
> Verifica que tots els fitxers del paquet compleixin el md5sum i timestamp del moment en que es van instal·lar. Permet verificar que no s'hagin modificat binaris dintre del sistema o comprovar si hem editat un fitxer de configuració en concret després de la instal·lacio.
> 
> **`equery depends paquet`**  
> Mostra els paquets instal·lats que tenen al paquet especificat com a dependència. Pot ser útil abans de desinstal·lar una llibreria, per saber si hi ha algun paquet que la està utilitzant i evitar problemes de dependències, ja que portage no ens avisa automàticament d'això.
> 
> **`equery hasuse useflag`**  
> Ens mostra els paquets que han estat compilats amb la USE _useflag_ activada. Per exemple pot servir per saber tot el que tenim compilat amb suport d'oss si volem canviar a alsa.
> 
> **`equery size paquet`**  
> Ens mostra el tamany que ocupa el paquet a disc i el nombre de fitxers que ha instal·lat.

Aquestes són les opcions que jo utilitzo mes a sovint, però <acronym title="man equery">en te d'altres</acronym>. Si encara no heu utilitzat mai equery veureu com poc a poc us anireu acostumant i es convertirà en una ferramenta indispensable per al manteniment del sistema de paquets.

