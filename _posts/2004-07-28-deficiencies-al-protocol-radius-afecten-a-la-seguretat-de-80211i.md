---
layout: post
title: Deficiències al protocol RADIUS afecten a la seguretat de 802.11i
date: 2004-07-28 16:59:37.000000000 +02:00
type: post
categories:
- security
- wireless
tags: []
meta:
permalink: "/2004/07/28/deficiencies-al-protocol-radius-afecten-a-la-seguretat-de-80211i/"
---
Fa dos dies va començar el que jo anomeno _rebombori mediàtic_ sobre una vulnerabilitat que suposadament afecta al nou [estàndard de seguretat 802.11i](/blog/2004/06/25/6/), podeu trobar més informació a [Daily Wireless](http://dailywireless.org/modules.php?name=News&file=article&sid=2864), [Security Pipeline](http://www.securitypipeline.com/network/25600432%3bjsessionid=ZWTWLX4N3P24SQSNDBGCKHY), [eWeek](http://www.eweek.com/article2/0,1759,1627229,00.asp) i [ebcvg](http://www.ebcvg.com/news.php?id=3073) entre d'altres.

Gràcies a que [l'Oriol](http://oriol.joor.net) m'ha enviat un email preguntant-me sobre el tema he investigat una mica més, així que aquí us presento una descripció una mica més tècnica que la que donen tots aquests articles i la meva versió dels fets.

La vulnerabilitat no està en el protocol 802.11i com [alguns titulars apunten](http://www.caballe.com/2004/07/27.html#a3068), sinó que radica en el fet de que al utilitzar aquest protocol els punts d'accés s'han de comunicar amb el servidor RADIUS per a autenticar als clients inalàmbrics. RADIUS és un protocol que funciona sobre UDP sense cap tipus d'encriptació, per tant susceptible a ser esnifat. La seguretat que ofereix el protocol RADIUS és pràcticament nula, ja que en lloc d'enviar la clau en clar el que es fa és enviar el MD5 d'aquesta. Això no és res nou, tots els que utilitzen RADIUS ho saben, i per això les comunicacions entre servidors RADIUS que han de viatjar a través d'Internet (per fer roamings entre operadores, per exemple) sempre es fan a través d'un túnel xifrat, normalment amb IPSec.

Aprofitant aquesta debilitat del protocol RADIUS, es on sorgeix la vulnerabilitat descoberta per [Aruba Wireless Networks](http://www.arubanetworks.com/), que curiosament és una empresa que es dedica a vendre conmutadors (_swich_) centralitzats per 802.11i que incorporen la encriptació en el propi switch en lloc d'en el punt d'accés (evitant la comunicació en clar entre l'AP i el RADIUS), per això lo del _rebombori mediàtic_ que comentava al principi, ja que ells mateixos se'n beneficiaran de la "vulnerabilitat" que han "descobert" venent els seus productes.

L'atac en sí permet que un atacant pugui interceptar les claus que s'utilitzaran per a encriptar la comunicació inalàmbrica entre el AP i el client, ja que aquestes claus son enviades pel servidor RADIUS al AP cada cop que un client s'autentica.

Per realitzar l'atac, l'atacant lògicament haurà de disposar d'accés físic a la part cablejada de la xarxa wireless (els diferents APs i el servidor RADIUS solen estar interconnectats amb cable) i poder esnifar dins d'aquesta xarxa (si la xarxa utilitza un switch s'haurà de fer amb ARP poisoning, per exemple amb el [ettercap](http://ettercap.sf.net)).

Com l'atacant té accés físic, pot col·locar un _rogue AP_ (un AP que no forma part de la xarxa real, sinó que es col·loca expressament per provocar que els clients legítims s'associïn al AP _maligne_ propietat de l'atacant). Aquest _rogue AP_ haurà de tenir configurat el _shared secret_ (clau secreta compartida entre els APs i el RADIUS) correcte per poder-se comunicar amb el servidor RADIUS (si no no podrà autenticar a cap client). Si l'atacant no coneix el shared secret haurà d'obtenir-lo realitzant un atac de força bruta, per tant això dificulta molt més l'atac i el fa molt més costós d'implementar. Existeixen utilitats com [Cain & Abel](http://www.oxid.it/cain.html) per automatitzar aquest procés.

Un cop l'atacant hagi aconseguit col·locar el _rogue AP_ correctament configurat (amb el _shared secret_) per poder comunicar-se amb el servidor RADIUS, haurà d'aconseguir que un client legítim (la víctima) s'associí a ell, per fer-ho pot enviar un paquet _deauthentication request_ al client legitim. Llavors, el client intentarà reautenticar-se i el _rogue AP_ enviarà la petició d'autenticació del client al servidor RADIUS que acceptarà al usuari i enviarà les claus de xifrat al _rogue AP_.

D'aquesta manera l'atacant haurà aconseguit les claus que xifraran la comunicació inalàmbrica a capa dos entre el _rogue AP_ i la víctima.

