---
layout: post
title: La VoIP està de moda
date: 2005-04-06 22:38:06.000000000 +02:00
type: post
categories:
- linux
- voip
- wireless
tags: []
meta:
permalink: "/2005/04/06/la-voip-esta-de-moda/"
---
<p>A <a href="http://voip-info.org">VoIP info</a> estan preparant una <a href="http://voip-info.org/tiki-index.php?page=bounty%20skype">donació</a> ---actualment superior a $1000--- per al primer que programe un mòdul per a <a href="http://www.asterisk.org/">Asterisk</a> que l'interconnecti amb <a href="http://www.skype.com">Skype</a>. Això permetria fer una pasarel·la SIP-Skype casolana amb moltes possibilitats, com per exemple utilitzar hardware purament SIP per a fer trucades amb SkypeOut, i la possibilitat de tenir la PBX sense necessitat de contractar línies telefòniques reals, sinó utilitzant els serveis de Skype.</p>
<p>El mòdul ha de funcionar amb Linux, com a extensió de Asterisk. Per a fer-lo hi ha dues possibilitats:</p>
<blockquote><p>
a) Que treballi amb el protocol propietari de Skype a baix nivell (prèviament s'ha de fer enginyeria inversa del protocol).<br />
b) Que estigui programat utilitzant la API de Skype per a Linux, que estarà disponible pròximament i <a href="http://forum.skype.com/viewtopic.php?t=9689">pel que sembla</a> estarà basada en <a href="http://www.freedesktop.org/Software/dbus">D-Bus</a>.
</p></blockquote>
<p>De moment però existeixen <a href="http://voip-info.org/wiki-VOIP+Service+Providers">alternatives</a> per a connectar un Asterisk amb la <acronym title="Public Switched Telephone Network">PSTN</acronym>, com per exemple <a href="http://www.voipjet.com">VoipJet</a> que s'interconnecta amb Asterisk amb el protocol <acronym title="Inter-Asterisk EXchange">IAX</acronym> i permet fer trucades amb un telèfon SIP a preus fins i tot <a href="http://www.voipjet.com/prices.php">més competitius</a> que els <a href="http://www.skype.com/products/skypeout/rates/all_rates.html">preus de SkypeOut</a>.</p>
<p>Una altra alternativa a considerar en el mon de la Veu-sobre-IP és la <a href="http://itsp.typepad.com/voip/2005/03/telfono_wifi_vo.html">nova oferta de PeopleCall</a> que sortirà pròximament al mercat però que ens avança Herme en el seu blog, es tracta d'un telèfon wifi que ens permet realitzar o rebre trucades a un número de telèfon espanyol amb les <a href="http://www.peoplecall.com/es/ESPEOP2913_01385_es.htm">tarifes de PeopleCall</a>.</p>
<p>Una altra idea molt interessant que <a href="http://www.skypejournal.com/blog/archives/2005/03/skypein_set_to.php">ens expliquen a Skype Journal</a>, és muntar un call center distribuït utilitzant Skype: Una de les característiques de Skype és que pots tenir varis clients oberts a la vegada (per exemple al portàtil i a la PDA) i atendre les trucades per on et sigui més convenient, si combinem això amb un número de telèfon <a href="http://www.skype.com/products/skypein/">SkypeIn</a> podem fer que amb un únic login (un únic usuari de Skype) entrin 50 persones --- per dir un número---
amb ordinadors diferents situats a llocs diferents (pensem en el teletreball...). Aquestes 50 persones rebran les trucades al número SkypeIn del call center i l'operador que no estigui ocupat atenent una altra trucada podrà contestar la que entra, això és gracies a que el número de SkypeIn no comunica (o salta al [Skype VoiceMail](http://www.skype.com/products/skypevoicemail/)) si hi ha un client disponible per atendre la trucada. Els 50 operadors també haurien de poder realitzar trucades simultàniament amb SkypeOut... No sé fins a quin punt pot ser viable la idea, si es fa ben fet utilitzant la [Skype API](http://www.skype.com/community/devzone/doc.html) i Skype no elimina la possibilitat de rebre trucades diferents a diferents clients connectats amb el mateix login el tema pot resultar interessant, sobre tot per la reducció de costos i possibilitats de teletreball que implicaria per al negoci dels call centers.

