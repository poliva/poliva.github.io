---
layout: post
title: Nova versió del classificador de QoS
date: 2004-10-22 12:26:16.000000000 +02:00
type: post
categories:
- linux
- pofHQ
tags: []
meta:
permalink: "/2004/10/22/nova-versio-del-classificador-de-qos/"
---
Ahir vaig estar jugant una estona més amb el [script per prioritzar](/blog/2004/10/20/87/) tràfic i li he fet unes quantes modificacions:

· He afegit una classe interior (pare) al downlink que conté les dues classes de menys prioritat (bulk i limited), d'aquesta manera aquestes dues classes nomes poden agafar amplada de banda del pare i permeten que la classe prioritaria sigui encara més prioritaria.

· He afegit els paràmetres burst i cburst a un valor bastant alt (12k) a la classe d'alta prioritat, la resta de classes obtenen el valor mínim possible calculat automàticament. D'aquesta manera les comunicacións amb VoIP envien més quantitat d'informació quan s'està enviant la ràfaga de tràfic de la cua més prioritaria i no es talla la veu si tenim el enllaç saturat per tràfic d'altres classes.

· He afegit una classe especial per al uplink de HTTP, aquesta classe permet que el tràfic que envía cap a internet el servidor web intern va ràpid encara que tinguem el bittorrent funcionant.

· Finalment, he afegit un exemple de com marcar el tràfic del joc on-line wolfenstein enemy territory, a petició del Daniel, el meu company de pis que vol tenir sempre el millor ping ;)

Aquí us podeu descarregar la nova versió: [QoS bandwidth classifier v1.1](/archives/files/bw-shaper1.1.sh).

