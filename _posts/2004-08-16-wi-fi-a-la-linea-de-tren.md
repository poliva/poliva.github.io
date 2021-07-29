---
layout: post
title: wi-fi a la linea de tren?
date: 2004-08-16 23:20:40.000000000 +02:00
type: post
categories:
- wireless
tags: []
meta:
  tags: ''
permalink: "/2004/08/16/wi-fi-a-la-linea-de-tren/"
---
<p>Es molt curiós, avui fent <acronym title="wardriving desde un regional express de RENFE">regional-express-driving</acronym> m'he trobat amb més de 50<br />
APs repartits al llarg del territori amb una serie de característiques comuns: tots al canal 6, en mode adhoc i<br />
amb el SSID "<b>north-east</b>", també m'ha estranyat que tots tenen el beacon<br />
interval canviat a 200 ms, quan per defecte bé a 100. I us puc asegurar que fa una setmana no hi eren ;)</p>
<p>N'hi ha des de la sortida de Sants estació fins a St. Vicenç de Calders,<br />
despres he deixat de veure'n una bona estona fins que he arribat a la estació<br />
de Tarragona, on n'he trobat un al entrar i un altre al sortir i uns quants més<br />
en el camí de Tarragona a Cambrils.</p>
<p>Un altre detall és que n'hi havia alguns que donaven fins i tot cobertura dintre dels túnels.</p>
<p>Si us fixeu en les MACs dels aparells veureu que segueixen una relació molt<br />
extranya, estan copiades en l'ordre en el que he anat trobant les xarxes,<br />
l'últim octet sempre és un "19", el penúltim octet va canviant successivament:<br />
88, 89, 8A, 8B, 8C... fins a 9E al arribar a St. Vicenç i després un salt fins<br />
a CC i CF als dos que hi ha a Tarragona i segueix de E3 fins a E7 a Salou i<br />
Cambrils.<br />
Els octets dos i quatre sempre són iguals, no es repeteixen en cap altre AP i<br />
el primer octet sempre és 00.</p>
<p>Em pregunto per a que deuen servir, estic segur que no són per a donar-nos<br />
accés a Internet als passatgers, ja que no hi ha ni molt menys cobertura en tot<br />
el trajecte, a més no crec que poguéssim fer <i>handoff</i> a 180Km/h, si volguessin<br />
fer això seria molt més raonable posar els APs dintre del tren, aquí teniu un<br />
exemple d'un dels APs que us dic:<!--more--></p>
<pre>
 SSID: north-east
 BSSID : 00:FA:30:FA:9C:19
 Carrier : IEEE 802.11g
 Manuf : Unknown
 Max Rate: 11.0
 Clients : 0
 Type: Ad-hoc
 Channel : 6
 WEP : No
 Beacon : 200 (0.204800 sec)
</pre>
<p>I aquí un <i>probe request</i> que també he capturat, amb una MAC que no segueix el<br />
patró de la resta:</p>
<pre>
 SSID : north-east
 BSSID : 00:0F:20:7F:45:EB
 Carrier : IEEE 802.11g
 Manuf : Unknown
 Max Rate: 11.0
 First : Mon Aug 16 19:43:41 2004
 Latest : Mon Aug 16 20:22:15 2004
 Clients : 1
 Type : Probe request (searching client)
</pre>
<p>A continuació la llista de MACs que comentava abans:</p>
<pre>
----barcelona----
00:22:B5:22:88:19
00:4F:3D:4F:88:19
00:00:F6:00:89:19
(probe) 00:0F:20:7F:45:EB
00:BE:76:BE:8A:19
00:C8:A6:C8:8B:19
00:7B:92:7B:8C:19
00:A8:1A:A8:8C:19
00:D4:6F:D4:8C:19
00:00:F6:00:8D:19
00:86:C1:86:8D:19
00:B3:15:B3:8D:19
00:BD:E1:BD:8E:19
00:16:8A:16:8F:19
00:6F:66:6F:8F:19
00:9C:21:9C:8F:19
00:C8:76:C8:8F:19
00:F4:FD:F4:8F:19
00:FF:92:FF:90:19
00:84:8F:84:91:19
00:B1:4A:B1:91:19
00:DD:9F:DD:91:19
00:09:F3:09:92:19
00:36:7A:36:92:19
00:63:02:63:92:19
00:41:77:41:93:19
00:F3:98:F3:93:19
00:2A:80:2A:95:19
00:83:5C:83:95:19
00:AF:E3:AF:95:19
00:8D:F3:8D:96:19
00:BA:7A:BA:96:19
00:E7:01:E7:96:19
00:28:4F:28:99:19
00:DA:A3:DA:99:19
00:06:F8:06:9A:19
00:8C:5C:8C:9A:19
00:FA:30:FA:9C:19
00:31:E7:31:9E:19
00:5E:3C:5E:9E:19
00:8A:C2:8A:9E:19
----tarragona----
00:86:04:86:CC:19
00:D4:E5:D4:CF:19
-----salou-------
00:73:42:73:E3:19
00:F8:3F:F8:E3:19
00:24:C6:24:E4:19
00:51:B4:51:E4:19
00:7E:3D:7E:E4:19
00:D7:4C:D7:E4:19
00:03:A1:03:E5:19
00:30:2A:30:E5:19
----cambrils-----
00:9D:FB:9D:E7:19
</pre>

Si algú sap alguna cosa de que pot ser això o te alguna idea que deixi un comentari i ens ilustre :)

