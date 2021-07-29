---
layout: post
title: Posant-li el turbo al Skype
date: 2005-02-01 00:30:06.000000000 +01:00
type: post
categories:
- linux
- voip
tags: []
meta:
permalink: "/2005/02/01/posant-li-el-turbo-al-skype/"
---
Una de les _features_ que m'agradaria que tingués [Skype](http://www.skype.com) i que no té és que directament marques el tràfic de les converses amb <acronym title="Type Of Service">ToS</acronym> _Minimize-Delay_, d'aquesta manera seria molt mes fàcil prioritzar-lo amb <acronym title="Quality of Service">QoS</acronym> fent servir per exemple [aquest script classificador](/blog/2004/10/22/nova-versio-del-classificador-de-qos/). Això permetria poder utilitzar programes _peer-to-peer_ al mateix temps que Skype en la mateixa màquina.

Fins ara la meva solució per a obtenir màxima prioritat en el tràfic de Skype es basa en donar-li màxima prioritat al meu portàtil (des-d'on faig la trucada) durant el temps que estic utilitzant Skype per parlar, i tornar-lo a deixar amb prioritat "normal" un cop finalitzada la trucada IP. D'aquesta manera, mentre estic parlant a través de Skype amb el portàtil no puc tenir el _peer-to-peer_ funcionant, encara que no em molesta si el té alguna altra màquina de la xarxa. Però com comprendreu la solució és lletja.

Quan vaig fer els scripts per a gestionar el QoS vaig estar buscant la manera de filtrar el tràfic de Skype i no ho vaig aconseguir, no segueix cap patró que ens permeti identificar-lo fàcilment utilitzant només iptables, però avui finalment he trobat la solució: [l7-filter](http://l7-filter.sourceforge.net/), un classificador de paquets per a Linux a nivell d'aplicació (capa 7).

Segons el que he llegit fins ara es tracta de _parxejar_ el kernel i les iptables, per poder utilitzar aquesta sintaxis:  
`-m layer7 --l7proto skype`, amb el [patró](http://l7-filter.sourceforge.net/layer7-protocols/protocols/skype.pat) que reconeix el tràfic de Skype, que encara no està massa depurat. Quan tingui temps per muntar-ho al firewall ja us contaré que tal funciona.

Val a dir que aquesta solució es d'aquelles de _matar mosques a canyonassos_, però ja que Skype ens ho vol posar difícil i no ens marca els paquets amb ToS com deia al principi del post, pues ens ho muntem nosaltres per identificar el tràfic de Skype a nivell d'aplicació parxejant el kernel i les iptables :)

