---
layout: post
title: Alarmisme i seguretat en les xarxes inalàmbriques
date: 2004-09-03 13:48:39.000000000 +02:00
type: post
categories:
- security
- wireless
tags: []
meta:
permalink: "/2004/09/03/alarmisme-i-seguretat-en-les-xarxes-inalmbriques/"
---
Acabo de llegir a [bandaancha](http://www.bandaancha.st) un article titulat [Grupo de expertos avisa sobre más de 200 sistemas WiFi desprotegidos en Tenerife](http://www.bandaancha.st/weblogart.php?artid=2734), on s'explica que uns _expertos en nuevas tecnologías_ han fet [wardriving](/blog/2004/08/24/53/) a les àrees metropolitanes de Gran Canaria i Las Palmas i tal com titula l'article han trobat més de 200 xarxes sense encriptació, descrivint la situació com un gran problema de seguretat a empreses i institucions públiques i deixant veure que les seves xarxes son insegures.

Sota el meu punt de vista aquest tipus de noticies provoquen l'alarmisme social entre els no iniciats, els que han sentit campanes sobre wireless, si mirem els [resultats de la última world wide wardrive](http://www.worldwidewardrive.org/wwwdstats.html), que ve a ser el mateix que han fet aquets _expertos_ a canaries però a nivell mundial veiem que hi ha 140.890 AP's (un 61,6%) descoberts sense encriptació. Vol dir això que totes aquestes xarxes són insegures? vulnerables? NO, i rotundament NO.

Trobar un AP sense encriptació no significa res dolent, no s'ha de tenir aquesta sensació alarmista, aquests AP's poden ser APs de operadors inalàmbrics WISPs que ofereixen accés a Internet **de pagament** , quan t'associes a l'AP apareix una plana de login on et demana un login i un password, i no pots navegar si no t'autentiques, però quan es fan aquestes estadístiques això no es veu, i es comptabilitza com una xarxa més sense encriptació. També hi ha comunitats d'usuaris com [Barcelona Wireless](http://www.barcelonawireless.net/), [Madrid Wireless](http://www.madridwireless.net/), [Mataró Wireless](http://www.matarowireless.net/), etc... on els usuaris monten xarxes urbanes paralel·les a Internet, i fins i tot podem trobar usuaris que desinteressadament comparteixen la seva connexió amb qualsevol persona que passi pels voltants, totalment conscients de que tenen un AP sense cap tipus d'encriptació.

Això no vol dir que les xarxes que hi ha al darrera d'aquests APs siguin insegures, no vol dir res. Per posar un exemple, jo a casa tinc dos APs, un amb WPA que funciona com a bridge i dona accés a la xarxa interna, evidentment aquest AP utilitza xifrat i no permet connexions de usuaris no legítims, però també tinc un altre AP totalment obert on si et connectes no pots accedir ni a Internet ni a cap altra màquina de la xarxa perquè està situat a una subxarxa separada, per a accedir a Internet prèviament t'has d'autenticar amb [OpenVPN](http://openvpn.sourceforge.net/), però de cara a les estadístiques, això també és un AP sense encriptació.

Crec que no hem de fer tant de cas tant cegament als números ni tant de alarmisme social, el que vulgui una xarxa inalàmbrica segura te maneres de fer-ho i el que vulgui tenir el AP obert està en tot el seu dret, i hem de ser conscients de que això no significa que el que hi ha al darrera sigui una xarxa insegura.

