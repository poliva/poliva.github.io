---
layout: post
title: QoS bandwidth classifier v1.2
date: 2005-12-12 03:02:11.000000000 +01:00
type: post
categories:
- linux
tags: []
meta:
  tags: QoS bandwidth classifier shaper HTB
permalink: "/2005/12/12/qos-bandwidth-classifier-v12/"
---
He penjat la versió 1.2 del script per prioritzar l'ample de banda, el tinc funcionant sense problemes al firewall de casa desde el mes d'octubre, però se m'habia oblidat penjar-la aquí i avui hi he pensat.

Incorpora tres canvis respecte a la [versió anterior](/blog/2004/10/22/nova-versio-del-classificador-de-qos/):

- La priorització per servei (PRIOPORT i LIMITEDPORT) ara permet discriminar ports TCP i UDP.
- Cálcul del valor óptim de _r2q_ per evitar situacions on el quantum és mes petit que el valor de MTU.
- Corregit un bug que feia que els hosts amb alta prioritat o amb ample de banda limitat no es tinguessin en compte

**Podeu descarregar-lo des d'aquí: [bw-shaper1.2.sh](/archives/files/bw-shaper1.2.sh).**

<!--more-->

El funcionament del classificador està explicat en els anuncis de les versions anteriors:

- [Versió 1.1](/blog/2004/10/22/nova-versio-del-classificador-de-qos/)
- [Versió 1.0](/blog/2004/10/20/qos-prioritzacio-damplada-de-banda/)

Es poden veure els resultats de forma gràfica [aquí](/graphs/). Les gràfiques estan generades amb [rrdtool](http://people.ee.ethz.ch/~oetiker/webtools/rrdtool/), tal com vaig explicar en [aquest post](/blog/2004/10/14/introducci-a-rrdtool/).

