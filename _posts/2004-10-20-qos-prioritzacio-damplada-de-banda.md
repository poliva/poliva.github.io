---
layout: post
title: 'QoS:  priorització d''amplada de banda'
date: 2004-10-20 23:59:36.000000000 +02:00
type: post
categories:
- linux
- pofHQ
tags: []
meta:
permalink: "/2004/10/20/qos-prioritzacio-damplada-de-banda/"
---
Finalment ahir a la nit vaig trobar una estona per _polir_ el script que utilitzo per a gestionar l'amplada de banda de la meva connexió a internet i fer-lo més genèric.

El script utilitza <acronym title="Hierachical token bucket">HTB</acronym>, i està dissenyat per a executar-se en una màquina dedicada a fer de router/firewall amb dues interfaces, una connectada a Internet i l'altra connectada a la LAN, si volem obtenir el millor rendiment aquesta màquina no ha de donar ningún servei.

A nivell introductori, això és el que fa el script:

Crea 3 classes per a la interfície interna, i 3 per a la interfície externa. Com amb HTB només podem controlar el tràfic que enviem utilitzarem la interfície externa per a controlar el downlink i la interna per a controlar el uplink.

NOTA: si només disposem d'una interfície s'hauria d'utilitzar [IMQ](http://www.linuximq.net/)

Les 3 classes son les següents:

**· Classe 1:10, màxima prioritat**

> En aquesta classe situem el tràfic interactiu (per exemple VoIP o ssh): els paquets marcats com a _minimum delay_ amb TOS (per exemple el SSH, però no el scp), els ACKs per a que els downloads vagin ràpid encara que tinguem el uplink saturat i el tràfic ICMP per a obtenir bons resultats al ping encara que el canal estigui saturat (tècnicament no és molt útil per que "ens enganya" quan mesurem la conectivitat, però permet impressionar als amics :P).

**· Classe 1:20, prioritat mitja**

> En aquesta classe situem el tràfic no interactiu, per defecte tot el tràfic va a parar aquí, a no ser que especifiquem el contrari.

**· Classe 1:30, prioritat baixa**

> Aquesta classe té el ample de banda limitat, jo la utilitzo principalment per limitar el tràfic HTTP que surt del meu servidor web cap a internet, així encara que algú s'estigui baixant coses del servidor web el canal de uplink no es satura completament. Aquesta classe no pot agafar ample de banda sobrant, encara que el canal no s'estigui utilitzant el tràfic http no passarà del limit assignat.

<!--more-->  
Per a utilitzar-lo primer hem de configurar les variables del principi:

· UPLINK i DOWNLINK indiquen la velocitat de l'enllaç, per exemple si disposem d'una ADSL de 512/128 haurem de posar aquests valors o una mica menys, per a no crear cues al ISP, el millor és començar en el valor real i anar baixant i fent proves fins que trobem el valor òptim per a la nostra connexió.

· DEV\_EXT i DEV\_INT serveixen per indicar els noms de les interfícies interna i externa

· PRIOPORT\_OUT és una llista de ports d'origen que surten de la nostra LAN i que volem que tinguin prioritat màxima, per entendre el concepte haurem de pensar així: _"tot el tràfic que surti de la meva LAN cap a Internet i que tingui com a port d'origen \<tal\> tindrà màxima prioritat"_

· PRIOPORT\_IN és una llista de ports d'origen que entren cap a la nostra LANi que volem que tinguin màxima prioritat, per entendre el concepte haurem de pensar així: _"tot el tràfic que entri cap a la meva LAN que arriba d'Internet i que tingui com a port d'origen \<tal\> tindrà màxima prioritat"_

· LIMITEDPORT\_OUT és igual que PRIOPORT\_OUT però anirà a parar a la classe amb l'ample de banda limitat, per tant tindrà prioritat mínima.

· LIMITEDPORT\_IN és igual que PRIOPORT\_IN però anirà a parar a la classe amb l'ample de banda limitat, per tant tindrà prioritat mínima.

· PRIOHOST es una llista de hosts als que volem donar sempre màxima prioritat per a tot el tràfic que generin, tant d'entrada com de sortida

· LIMITEDHOST és una llista de hosts als que volem limitar sempre l'ample de banda (per exemple si tenim algun usuari d'aplicacions d'estil emule o bittorrent a la nostra xarxa)

Finalment, si volem modificar el tràfic que podrà obtenir cada cua haurem de modificar els valors de RATE i CEIL: RATE controla el ample de banda mínim que s'intentarà donar a la classe en cas de que el canal estigui saturat i CEIL especifica el màxim ample de banda que podrà obtenir aquella classe (no podrà agafar ample de banda --encara que no estigui utilitzant-se el canal-- per sobre del valor de CEIL).  
Per a fer-ho genèric aquests valors s'especificaran a la configuració del script en forma de fracció, així per exemple si especifiquem aquests valors:

`
LIMITED_UP_RATE="1/8"
LIMITED_UP_CEIL="1/2"
`

i hem especificat `UPLINK="270"` la classe amb ample de banda limitat de uplink obtindrà 270/8 kbits garantits i podrà consumir 270/2 kbits com a màxim; el script ja s'encarregarà de calcular el valor adequat per al RATE i el CEIL de la classe.

Aquí us podeu descarregar el script: [QoS bandwidth classifier v1.0](/archives/files/bw-shaper1.sh).

