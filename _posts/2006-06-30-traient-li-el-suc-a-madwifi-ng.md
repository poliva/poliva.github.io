---
layout: post
title: Traient-li el suc a madwifi-ng
date: 2006-06-30 18:58:57.000000000 +02:00
type: post
categories:
- linux
- wireless
tags: []
meta:
  tags: madwifi madwifi-ng atheros wifi wi-fi wireless linux wmm wme 802.11e 802.11h
    VAP
permalink: "/2006/06/30/traient-li-el-suc-a-madwifi-ng/"
---
En [Brainstorm](http://brainstorm.nopcode.org) va [parlar](http://blogs.nopcode.org/brainstorm/2006/06/27/madwifi-ng-i-els-vaps-2/) fa poc dels <acronym title="Virtual Access Point">VAPs</acronym> que es poden crear amb [madwifi-ng](http://madwifi.org/), per no repetir el seu artícle intentaré extendre'l una mica amb algunes coses interessants que també podem fer amb la _next generation_ de madwifi.

### Virtual Access Points

Els VAPs són la característica més interessant de madwifi-ng, podem crear amb una única interface física APs virtuals amb configuracions diferents, però tots usaràn sempre la mateixa frequència (canal). Podem crear diversos VAPs treballant en mode punt d'accés i un sol VAP treballant en mode client per interface fisica. Si el VAP que treballa en mode client és el primer que creem, no podrém crear més VAPs en aquella interface.

<!--more-->

Per crear els VAPs s'utilitza la comanda `wlanconfig` del paquet <tt>madwifi-ng-tools</tt>, resumint la sintaxis és la següent:

`wlanconfig ath create wlandev wifi0 wlanmode <ap|sta> [bssid|-bssid] [nosbeacon]`

Amb `wlanmode ap` creem VAPs en mode Master (punt d'accés) i amb `wlanmode sta` en mode client (station). Existeixen altres modes (monitor, wds...) però no entraré en tant de detall, per més informació mirar el man o la [documentació](http://madwifi.org/wiki/UserDocs) de madwifi-ng.

Amb la opció `-bssid` forçem a que el VAP utilitze l'adreça MAC de la eeprom del dispositiu físic, i amb la opció `bssid` es genera una adreça MAC automàticament.

Quan tenim VAPs en mode Master (AP) i en mode client (STA) sobre un mateix dispositiu físic, s'ha d'utilitzar el flag `nosbeacon` quan es crea el client, aquest flag deshabilita l'us dels _beacon timers_ per hardware per a treballar en mode client.

Si voleu veure alguns exemples us adreço al [artícle d'en Brainstorm](http://blogs.nopcode.org/brainstorm/2006/06/27/madwifi-ng-i-els-vaps-2/) que he comentat anteriorment.

#### Repetidor utilitzant VAPs

A continuació explicaré com fer un repetidor utilitzant VAPs, que de vegades pot ser bastant útil.

Suposem que tenim un punt d'accés amb SSID "prova" i que volem extendre l'area de cobertura, per fer-ho crearem dos VAPs. El primer VAP en mode AP servirà per donar cobertura als clients en l'area que nosaltres "extenem", el segon VAP en mode client servirà per associar-se al AP inicial que volem extendre.

Com tindrem un AP i un client sobre el mateix dispositiu físic, haurem de crear primer el AP i després el client i utilitzar `nosbeacon` quan creem el client, però quan aixequem les interfaces el VAP client s'ha de pujar primer i permetre-li associar-se al AP "prova" ja que aquest VAP serà el que seleccionarà el canal del AP existent.

```
# wlanconfig ath create wlandev wifi0 wlanmode ap ath0 # wlanconfig ath create wlandev wifi0 wlanmode sta nosbeacon ath1 # iwconfig ath0 essid "prova" # iwconfig ath1 essid "prova" # iwpriv ath0 wds 1 # iwpriv ath1 wds 1 # ifconfig ath1 up (Esperar a que s'associe al AP) # ifconfig ath0 up # ifconfig eth0 up # brctl addbr br0 # brctl addif br0 eth0 # brctl addif ath0 # brctl addif ath1 # brctl setfd br0 1 # ifconfig br0 up
```

Nota: En aquest exemple el nostre AP no podrà rebre tràfic IP perqué no li hem assignat cap adreça al bridge, si volem que pugi rebre tràfic podem assignar-li una IP, però el AP "prova" ha de supportar WDS i saber gestionar trames amb 4 adresses MAC.

### Seleccionar el mode d'operació

`iwpriv ath0 mode <mode>`  
Els diferents modes d'operació son:

| **Mode** | **Number** | **Description** |
| auto | 0 | mode d'operació automàtic |
| 11a | 1 | 802.11a (5GHz) - 54Mbps |
| 11b | 2 | 802.11b (2.4GHz) - 11Mbps |
| 11g | 3 | 802.11b/g (2.4GHz) - 54Mbps - compatible amb 802.11b |
| fh | 4 | 802.11 frequency hopping mode |
| 11adt/111at | 5 | 802.11a (5GHz) - dynamic turbo mode |
| 11gdt/11gt | 6 | 802.11g (2.4GHz) - dynamic turbo mode (108Mbps) |
| 11ast | 7 | 802.11a (5GHz) - static turbo mode |

Si volem treballar només amb mode 802.11g estrícte, sense permetre que els clients 802.11b es puguin connectar (deshabilita els rates inferiors a 6Mbps) ho podem fer amb la següent comanda:

`iwpriv ath0 pureg 1`

### Amagar el SSID

De vegades ens interessa no publicar _beacon frames_ anunciant el SSID de la xarxa, per fer-ho utilitzarem la següent comanda:

`iwpriv ath0 hide_ssid 1`

### Canviar el temps entre beacons

Per defecte el SSID de la xarxa s'anuncia cada 100ms, podem canviar aquest valor amb la següent comanda:

`iwpriv ath0 bintval <tems en ms>`

### Habilitar suport de 802.11h

802.11h s'utilitza per evitar problemes d'interferència amb altres dispositius que utilitzen la mateixa frequència, particularment radars i equips mèdics quan treballem a 5GHz.

Per reduir la interferència s'utilizen dues tècniques:

- Dynamic Frequency Selection (DFS): el xip wifi detècta la presència d'altres dispositius i si solapaen el canal on està treballant l'AP actualment, automàticament canvia de canal.
- Transmit Power Control (TPC): el xip wifi redueix la poténcia d'emissió a un nivell que minimitza el risc d'interferència des-de i cap a altres dispositius.

Per habilitar el suport de 802.11h utilitzarem la següent comanda:

`iwpriv ath0 doth 1`

Si volem generar un _reassociation request_ utilitzarem la següent comanda:

`iwpriv ath0 doth_reassoc 1`

Quan utilitzem 802.11h la potència d'emissió pot variar entre un mínim predefinit per l'usuari i el màxim permes pel _regulatory domain_, per definir aquest límit utilitzarem la següent comanda:

`iwpriv ath0 doth_pwrtgt <potència>`

La potència s'ha d'especificar en escalons de 0.5 dBm, per exemple si volem que la potència d'emissió al canal actual pugui variar entre 13 dBm i el màxim permes pel _regulatory domain_ especificaríem el valor `26` com a potència en la comanda anterior.

### ACLs basades en adreces MAC

Madwifi-ng permet gestionar ACLs basades en MAC desde el propi driver, per fer-ho utilitzarem les següents comandes:

Primer hem de seleccionar de quina manera volem fer la gestió d'ACLs:

`iwpriv ath0 maccmd <mode>`

On el mode pot ser:

| **Argument** | **Action** |
| 0 | Deshabilita el control d'ACLs |
| 1 | Només permet accés a les MACs incloses en la llista d'ACLs |
| 2 | Només denega accés a les MACs incloses en la llista d'ACLs |
| 3 | Esborra la base de dades d'ACLs |
| 4 | Elimina la política d'ACLs |

Per afegir o esborrar adreces MAC a la llista d'ACLs s'utilitzen les comandes `add_mac` i `del_mac`, de la següent manera:

`iwpriv ath0 add_mac 00:01:02:03:04:05`

`iwpriv ath0 del_mac 00:01:02:03:04:05`

### Wireless Multimedia Extensions

Wireless Multimedia Extensions (WME) o Wi-Fi Multimedia (WMM) son una porció de les especificacions del estàndar 802.11e que proporcionen capacitats bàsiques de <acronym title="Quality of Service">QoS</acronym> a les xarxes 802.11 a nivell MAC (capa 2) prioritzant el tràfic en base a 4 categoríes o classes d'acces (<acronym title="Access Categories">AC</acronym>), encara que no proporcionen la capacitat de garantir throughput.

Per defecte WMM ve habilitat en madwifi-ng, podem habilitar-lo o deshabilitar-lo amb la següent comanda:

Habilitar: `iwpriv ath0 wmm 1`

Deshabilitar: `iwpriv ath0 wmm 0`

Les ACs que defineix l'estàndard de WMM són les següents:

| **#AC** | **Descripció** |
| 0 | BE - Best Effort |
| 1 | BK - Background |
| 2 | VI - Video |
| 3 | VO - Voice |

Els parámetres que veurem a continuació segueixen la següent sintàxi:

- 

`#AC` especifíca el nombre d'AC (de la taula anterior) al que volem modificar el paràmetre.

- 

`X`: Pot pendre dos valors, `0` o `1`. El valor `0` indica que modificarem els valors per al set de paràmetres d'AP, el valor `1` indica que ho farem per al set de paràmetres de client.

- 

`Y`: El valor que volem modificar en les unitats tal com es descriuen al estàndar de WMM.

Modificar els valors màxim i minim de la _contention window_: paràmetres `cwmin` i `cwmax`

`iwpriv ath0 cwmin <#AC> <X> <Y>`

`iwpriv ath0 cwmax <#AC> <X> <Y>`

Modificar el _Arbitration Inter Frame Spacing_ (AIFS): paràmetre `aifs`

`iwpriv ath0 aifs <#AC> <X> <Y>`

Modificar el tamany de la _Transmit Opportunity_ (TxOp): paràmetre `txoplimit`

`iwpriv ath0 txoplimit <#AC> <X> <Y>`

Modificar el flag _Admission Control Mandatory_ (ACM): paràmetre `acm`

`iwpriv ath0 acm <#AC> <X> <0|1>`

Si el últim paràmetre val `0` no és necessari el control d'admissió, si val `1` és obligatori.

Modificar la política de _NoACK_ (ACM): paràmetre `noackpolicy`

`iwpriv ath0 noackpolicy <#AC> 0 <0|1>`

El segón parámetre sempre ha de ser 0, ja que la política de NoACK només aplica al set de paràmetres d'AP i no de client.

Si el últim paràmetre val `0` la política que s'aplica és assumir que un client està actiu si retorna un ACK després d'enviar-li una _<acronym title="poll frame: trama que s'utilitza per demanar als clients si estan actius o no">trama de poll</acronym>_ i desassociar-lo / desautenticar-lo si no contesta. Si val `1` no s'aplica cap política.

### Altres històries

Com veieu el tema és bastant extens i si hagués de documentar totes les opcions i possibilitats no acabaríem mai, per això podeu llegir la magnífica [users guide](http://madwifi.org/users-guide/users-guide.html) i la [wiki de madwifi](http://madwifi.org/wiki/UserDocs) on trobareu altres exemples i documentació.

