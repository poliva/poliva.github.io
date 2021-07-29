---
layout: post
title: El proxy de telefònica permet fer portscans anònims
date: 2004-07-01 10:29:01.000000000 +02:00
type: post
categories:
- security
tags: []
meta:
permalink: "/2004/07/01/el-proxy-de-telefnica-permet-fer-portscans-annims/"
---
Curiós i senzill, però efectiu el mètode que descriu Ripe en l'article  
[Uso del proxy de  
 Telefónica for fun and profit](http://www.7a69ezine.org/node/view/101) publicat a [7a69ezine](http://www.7a69ezine.org): bàsicament només cal fer  
 una connexió al port 80 de qualsevol host, aquesta petició serà  
 automàticament capturada pel proxy transparent de la timo, llavors li  
 passem per GET el host i el port on volem accedir, per exemple _GET  
 http://www.7a69ezine.org:22 HTTP/1.0_ ens tornarà el banner del  
 servidor SSH i tancarà la connexió.

