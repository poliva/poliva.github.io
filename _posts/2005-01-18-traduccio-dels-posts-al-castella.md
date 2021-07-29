---
layout: post
title: Traducció dels posts de WordPress al castellà
date: 2005-01-18 17:47:13.000000000 +01:00
type: post
categories:
- pofHQ
tags: []
meta:
permalink: "/2005/01/18/traduccio-dels-posts-al-castella/"
---
[Varies](http://www.gilsblog.name/) [persones](http://junyent.no-ip.org/) m'han demanat com ho faig per a traduir automàticament el contingut dels posts al castellà, així que faig un post explicant _el secret_ ;)

El sistema és molt simple, utilitzo el traductor d'[Internostrum](http://www.internostrum.com/) (ho podeu veure si mireu el _footer_ de la pàgina traduïda) i el que faig és un bucle per la pàgina original (sense traduir) i vaig imprimint línia per línia fins que arribo al post, un cop arribo al post imprimeixo el contingut que em retorna internostrum, canviant alguns dels errors mes comuns amb la funció `ereg_replace()` de php, i quan s'acaba el post torno a imprimir la web original.

Podeu veure el codi font del fitxer que fa la traducció [aquí](/cat2es.phps). Per a que surti l'enllaç de _castellano_ al final de cada post, s'ha de modificar la funció `comments\_popup\_link()` del fitxer  
`template-functions-comment.php` afegint aquestes línies al final:

```
echo "&nbsp;\<a href="/cat2es.php?url="; comments\_link(); echo ""\>Castellano\</a\>";
```

També es pot utilitzar el mateix _truc_ per a tenir un [feed RSS en castellà](/feed-es.php), però això ho deixo com a exercici de programació per al lector ;)

