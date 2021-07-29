---
layout: post
title: Capturar les credencials d'un roadwarrior sense usar un rogue AP
date: 2006-07-02 13:10:10.000000000 +02:00
type: post
categories:
- linux
- security
- wireless
tags: []
meta:
  tags: wlan wi-fi security dhcp dns attack captive+portal rogue+ap
permalink: "/2006/07/02/capturar-les-credencials-dun-roadwarrior-sense-usar-un-rogue-ap/"
---
Un atac molt típic en entorns de hotspot que utilitzen un [portal captiu](http://en.wikipedia.org/wiki/Captive_portal) (x ej: [chillispot](http://www.chillispot.org/) o [nocat](http://nocat.net/)) és montar un _rogue AP_, es a dir, un punt d'accés que publique el mateix SSID que la xarxa legítima però controlat per nosaltres, generalment amb més potència que els APs legítims per provocar que els clients s'associen a nosaltres, o anar enviant trames de _deauth_ als clients connectats per a que tornen a scanejar fins que s'associen al nostre rogue AP.

Quan tenim als clients associats al nostre AP és molt fácil redirigir-los a un servidor web pròpi que serveixi una pàgina exactament igual que la pàgina de login del portal captiu, i fer creure al client que realment està autenticant-se a la xarxa legítima. D'aquesta manera podem capturar les seves credencials d'accés o fins i tot robar-li les dades bancaries en el cas de realitze una compra on-line.

Podem realitzar un atac molt semblant però sense la necessitat d'utilitzar un rogue AP per "enganyar" al client, la idea és bastant simple: Hem d'utilitzar dues interfaces de xarxa, una en mode monitor on posarem un sniffer que monitoritze les peticions DHCP de clients nous per capturar la IP que el servidor de DHCP els assigna. Quan el nou client obté l'adreça IP, per l'altra interficie redirigirem totes les seves peticions de DNS a la nostra IP on tenim el servidor web simulant la pàgina de login del portal captiu per capturar-li les credencials.

<!--more-->

### Implementació del atac

A continuació us explico una forma d'implementar l'atac utilitzant [madwifi-ng](http://madwifi.org/), [tethereal](http://www.ethereal.com/docs/man-pages/tethereal.1.html) i [dnsa-ng](http://www.packetfactory.net/projects/dnsa/):

Primer creem dos VAPs, un en mode monitor i l'altre en mode client. Associem la interface en mode client a la xarxa legítima:

```
# wlanconfig ath0 create wlandev wifi0 wlanmode monitor bssid # wlanconfig ath1 create wlandev wifi0 wlanmode sta bssid # iwconfig ath1 essid "any" # ifconfig ath1 up # ifconfig ath0 up
```

Seguidament utilitzarem l'script [wlan\_webauth.pl](http://packetstormsecurity.org/wireless/wlan_webauth.txt) de [Craig Heffner](http://www.craigheffner.com/security/), que automatitza l'atac de manera molt elegant:

```
./wlan\_webauth.pl ath0 192.168.1.70 ath1
```

En aquest exemple, 192.168.1.70 és la IP on tenim el servidor web, que generalment serà el nostre portàtil (interface client ath1). I ara només cal esperar a que un nou client es conecti :)

### Com evitar l'atac

Des del punt de vista d'un <acronym title="Wireless Internet Service Provider">WISP</acronym> és molt fàcil evitar que aquest tipus d'atac es pugui dur a terme, només cal fer que no sigui possible la comunicació entre diferents clients a partir de capa 2. Si el hotspot només té un punt d'accés haurem d'habilitar la opció _layer 2 user isolation_ o similar, que només permet comunicació entre client i punt d'accés, pero no entre clients connectats a un mateix AP. En Cisco aquesta opció es coneix com a <acronym title="Public Secure Packet Forwarding">PSPF</acronym>, en linksys es diu _Wireless Client isolation mode_, i cada fabricant li diu d'una manera.

Si tenim més d'un punt d'accés haurem de tenir en compte també que la comunicació entre clients connectats a diferents APs no sigui possible, per fer-ho haurem de tenir un switch gestionable de capa 3, que permeti habilitar la opció de que no hi hagi comunicació entre els diferents ports del switch on tinguem connectats els diferents APs, podem implementar-ho de varies maneres, per exemple una VLAN separada per a cada AP o amb la opció de _port protected_ si tenim un switch Cisco.

