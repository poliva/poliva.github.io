---
layout: post
title: Configuració GPRS a la i-mate PDA2k
date: 2004-11-22 01:03:04.000000000 +01:00
type: post
categories:
- gadgets
- HTC
- wireless
tags: []
meta:
  enclosure: |-
    /blog/wp-content/pda2k/image030.png
    10663
    image/png
  tags: ''
permalink: "/2004/11/22/configuracio-gprs-a-la-i-mate-pda2k/"
---
<p>Tot i que el <acronym title="General Packet Radio Service">GPRS</acronym> no és una tecnología barata, molts cops es la única alternativa que tenim per poder-nos connectar a Internet. A continuació explico com configurar la PDA2k per connectar amb GPRS utilitzant AMENA, VODAFONE i MOVISTAR.</p>
<p><!--more--></p>
<p>Des del menú d'inici anem a <em>Settings</em> i seleccionem la pestanya <em>Connections</em>:</p>
<p><img src="{{ site.baseurl }}/assets/images/2004/11/image001.png" alt="connections" class="center" /></p>
<p>Seleccionem la icona <em>Connections</em> i sota de "<em>My Work Network</em>" fem click a "<em>Add a new modem connection</em>", posem un nom qualsevol que identifiqui la connexió i seguidament seleccionem "<em>Cellular Line (GPRS)</em>":</p>
<p class="center"><img src="{{ site.baseurl }}/assets/images/2004/11/image003.png" alt="Amena" /><br />
<img src="{{ site.baseurl }}/assets/images/2004/11/image032.png" alt="Vodafone" /><br />
<img src="{{ site.baseurl }}/assets/images/2004/11/image021.png" alt="Movistar" /></p>
<p>A continuació clickem "Next" i configurem el <acronym title="Access Point Name">APN</acronym> amb el valor corresponent a la operadora que volguem configurar:</p>
<table class="taula" summary="APN">
<thead>
<tr>
<td><strong>Companyia</strong></td>
<td><strong>APN</strong>*</td>
</tr>
</thead>
<tbody>
<tr>
<td>Amena</td>
<td>amenawap</td>
</tr>
<tr>
<td>Vodafone</td>
<td>airtelnet.es</td>
</tr>
<tr>
<td>Movistar</td>
<td>movistar.es</td>
</tr>
</tbody>
</table>
<p class="center">*Actualment aquesta configuració es vàlida tant per a contracte com per a prepagament</p>
<p class="center">
<img src="{{ site.baseurl }}/assets/images/2004/11/image005.png" alt="apn amena" /><br />
<img src="{{ site.baseurl }}/assets/images/2004/11/image034.png" alt="apn vodafone" /><br />
<img src="{{ site.baseurl }}/assets/images/2004/11/image023.png" alt="apn movistar" /></p>
<p>Seguidament hem d'introduïr el nom d'usuari i la contrasenya:</p>
<table class="taula" summary="GPRS Username and Password">
<thead>
<tr>
<td><strong>Companyia</strong></td>
<td><strong>User name</strong></td>
<td><strong>Password</strong></td>
</tr>
</thead>
<tbody>
<tr>
<td>Amena</td>
<td>CLIENTE</td>
<td>AMENA</td>
</tr>
<tr>
<td>Vodafone</td>
<td>VODAFONE</td>
<td>VODAFONE</td>
</tr>
<tr>
<td>Movistar</td>
<td>MOVISTAR</td>
<td>MOVISTAR</td>
</tr>
</tbody>
</table>
<p class="center">
<img src="{{ site.baseurl }}/assets/images/2004/11/image007.png" alt="auth amena" /><br />
<img src="{{ site.baseurl }}/assets/images/2004/11/image036.png" alt="auth vodafone" /><br />
<img src="{{ site.baseurl }}/assets/images/2004/11/image025.png" alt="auth movistar" /></p>
<p>Anem a <em>Advanced...</em> i activem la compressió marcant les caselles "<em>Use software compression</em>" i "<em>Use IP header compression</em>":</p>
<p><img src="{{ site.baseurl }}/assets/images/2004/11/image019.png" alt="compression" class="center" /></p>
<p>Amb Vodafone i Movistar ja hem acabat, si tenim Amena prepagament haurem de configurar un proxy, ja que el GPRS d'Amena per a tarjetes prepagament només permet navegar per pàgines WAP i per HTTP i HTTPS a través d'un proxy :(</p>
<p>Per configurar el proxy anem a la pestanya "<em>Proxy Settings</em>", seleccionem les dues caselles i posem com a proxy server <code>wap.amenate.com</code>:</p>
<p><img src="{{ site.baseurl }}/assets/images/2004/11/image009.png" alt="proxy amena" class="center" /></p>
<p>Seguidament fem click a "<em>Advanced...</em>" i configurem els següents valors:</p>
<p><img src="{{ site.baseurl }}/assets/images/2004/11/image011.png" alt="proxy amena 2" class="center" /></p>
<table class="taula" summary="Amena Proxy Settings">
<thead>
<tr>
<td><strong>Protocol</strong></td>
<td><strong>Server</strong></td>
<td><strong>Port</strong></td>
</tr>
</thead>
<tbody>
<tr>
<td>HTTP</td>
<td>wap.amenate.com</td>
<td>8080</td>
</tr>
<tr>
<td>WAP</td>
<td>10.132.61.10</td>
<td>9201</td>
</tr>
<tr>
<td>Socks</td>
<td>wap.amenate.com</td>
<td>1080</td>
</tr>
</tbody>
</table>
<p>A continuació per connectar  a una pàgina WAP haurem de posar <strong>wsp://</strong> abans de l'adreça, i http:// o https:// per a pàgines HTTP i HTTPS.  Amb amena prepagament no podem fer res mes, ni ssh, ni pings ni res... ens assigna una IP privada i només ens permet sortir pel proxy.</p>
<p class="center">
<img src="{{ site.baseurl }}/assets/images/2004/11/image013.png" alt="gprs connection" /><br />
<img src="{{ site.baseurl }}/assets/images/2004/11/image015.png" alt="gprs connection 2" /><br />
<img src="{{ site.baseurl }}/assets/images/2004/11/image017.png" alt="ip settings" /></p>
<p>Per sort amb Movistar i Vodafone la cosa canvia, obtenim una IP pública i tenim connexió directa a Internet ---tant si som usuaris de prepagament com de contracte---
aixi que ara si que podem accedir a qualsevol servei d'internet (correu, ssh, etc...):

![ip settings 2]({{ site.baseurl }}/assets/images/2004/11/image028.png)  
 ![ip settings 3]({{ site.baseurl }}/assets/images/2004/11/image030.png)

Finalment, cal recordar-vos els preus del GPRS, que no es plan d'anar abusant:

> - Amb [Amena](http://www.amena.com/presentacion/particulares/gprs/tarifas.html) i amb [Vodafone](http://www.vodafone.es/Vodafone/ParticularesPS/ParticularesPS/0,2603,8788,00.html) el preu és de 20,48 ?/Mb + IVA (sense cap abonament).
> - Amb [Movistar](http://www.movistar.com/activa/servicios/acceso-internet/internet-gprs.htm) surt una mica més econòmic, a 15,36 ?/Mb + IVA (sense abonament), però amb un cost addicional de 10 centims per cada connexió que fem.

