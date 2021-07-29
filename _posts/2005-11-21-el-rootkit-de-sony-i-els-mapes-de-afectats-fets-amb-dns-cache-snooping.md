---
layout: post
title: El rootkit de Sony i els mapes d'afectats fets amb DNS Cache Snooping
date: 2005-11-21 01:25:15.000000000 +01:00
type: post
categories:
- security
tags: []
meta:
  tags: dns cache snooping sony rootkit map drm
permalink: "/2005/11/21/el-rootkit-de-sony-i-els-mapes-de-afectats-fets-amb-dns-cache-snooping/"
---
![Rootkit Sony]({{ site.baseurl }}/assets/images/2005/11/sony-rootkit.png)

Fa un temps que corre la noticia de que [Sony](http://www.sony.com/) ha distribuït CD's d'àudio amb un sistema de <acronym title="Digital Rights Management">DRM</acronym> que només permet que el CD es pugui reproduir amb el propi reproductor d'àudio que instal·la el mateix el CD. Aquest software, al mateix temps instal·la un rootkit al PC en qüestió i canvia els drivers del CD-Rom per evitar que es pugin fer copies del CD. Si voleu seguir la història completa recomano llegir el seguiment fet a [BoingBoing](http://www.boingboing.net/): [Part I](http://www.boingboing.net/2005/11/14/sony_anticustomer_te.html) i [Part II](http://www.boingboing.net/2005/11/17/sony_rootkit_roundup.html).

La part interessant de la notícia sota el meu punt de vista no és el rootkit ni la part legal o moral de l'assumpte, sinó aquests mapes que han sortit publicats a diferents llocs on es poden veure les parts del mon afectades pel rootkit:

- [USA](http://www.doxpara.com/planetsony2_usa.jpg)
- [Europa](http://www.doxpara.com/planetsony2_europe.jpg)
- [Japó](http://www.doxpara.com/planetsony2_japan.jpg)

Quan vaig veure els mapes, de seguida em vaig preguntar quina tècnica hauran seguit per detectar l'origen del rootkit, doncs per fi he trobat la resposta, i l'explica Dan Kaminsky, el creador dels mapes al seu [blog](http://www.doxpara.com/):

> Sony has a rootkit.  
> The rootkit phones home.  
> Phoning home requires a DNS query.  
> DNS queries are cached.

La tècnica que ha utilitzat Dan per fer els mapes s'anomena _DNS Cache Snooping_ i està molt ben detallada i explicada en [aquest _paper_ de Luis Grangeia](http://www.sysvalue.com/papers/DNS-Cache-Snooping/). Bàsicament consisteix en fer una query no recursiva al major nombre possible de servidors DNS arreu del mon, per saber si tenen el domini de Sony que intenta resoldre el rootkit al caché, per després geoposicionar aquests servidors de DNS amb [IP2Location](http://www.ip2location.com/).

