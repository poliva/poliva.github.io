---
layout: post
title: Indagacions sobre el protocol de Skype
date: 2005-01-21 13:12:38.000000000 +01:00
type: post
categories:
- security
- voip
tags: []
meta:
  enclosure: |-
    /blog/wp-content/skype.png
    6476
    image/png
  tags: ''
permalink: "/2005/01/21/indagacions-sobre-el-protocol-de-skype/"
---
![skype logo]({{ site.baseurl }}/assets/images/2005/01/skype.png)Fa uns dies a [Kriptopolis](http://www.kriptopolis.org/node/249) es qüestionava la seguretat de [Skype](http://www.skype.com), ja que teòricament utilitza RSA per a intercanvi de claus i AES per al xifrat de les dades, però com el protocol no és obert ens hem de fiar del que diuen els seus desenvolupadors i no es pot comprovar que la implementació sigui la correcta. Moltes empreses son reticents al ús de skype per que els preocupa la seva seguretat, per això [Simson Garfinkel](http://www.simson.net/blog/) va fer un informe analitzant la seguretat del protocol (sense conèixer-ne les especificacions), que es pot descarregar [aquí](http://www.soros.org/initiatives/information/articles_publications/articles/security_20050107/OSI_Skype5.pdf).

Ahir a [Slashdot](http://it.slashdot.org/article.pl?sid=05/01/20/1653217) ens anunciaven un altre estudi que dóna una explicació general de com funciona el protocol de Skype i una explicació més profunda sobre la capacitat de traspassar <acronym title="Network Address Translation">NAT</acronym> i els algoritmes de compressió d'àudio que utilitza Skype. Ha estat realitzat pel departament d'informàtica de la universitat de Columbia i es pot descarregar [aquí](http://www.cs.columbia.edu/~library/TR-repository/reports/reports-2004/cucs-039-04.pdf).

