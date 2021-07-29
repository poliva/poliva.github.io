---
layout: post
title: Asterisk + VoipJet
date: 2005-04-21 02:18:20.000000000 +02:00
type: post
categories:
- linux
- voip
tags: []
meta:
permalink: "/2005/04/21/asterisk-voipjet/"
---
<p>Avui m'he donat d'alta a <a href="http://www.voipjet.com/">VoipJet</a>, un servei de trucades per veu sobre IP similar a <a href="http://www.skype.com/products/skypeout/">SkypeOut</a>, però amb <a href="http://www.voipjet.com/prices.php">preus</a> més competitius:</p>
<table class="taula" summary="Preus SkypeOut VoipJet">
<thead>
<tr>
<td><strong>Destí</strong></td>
<td><strong><a href="http://www.skype.com/products/skypeout/rates/all_rates.html">SkypeOut</a></strong></td>
<td><strong><a href="http://www.voipjet.com/prices.php">VoipJet</a></strong></td>
</tr>
</thead>
<tbody>
<tr>
<td>Fixe Espanya</td>
<td>2</td>
<td>1,14</td>
</tr>
<tr>
<td>Móbil Espanya</td>
<td>26,9</td>
<td>18,07</td>
</tr>
<tr>
<td>USA</td>
<td>2</td>
<td>0,99</td>
</tr>
</tbody>
</table>
<p>NOTA: Els preus estàn en convertits a centims d'euro per minut, amb IVA inclòs</p>
<p>El pagament es fa a través de <a href="http://www.paypal.com">PayPal</a> i quan et dones d'alta et regalen 0,25 centims de dolar per a que pugis provar el servei. A diferència de SkypeOut et tarifiquen per segón en lloc de per minut, i de la mateixa manera que a SkypeOut no pagues si no s'estableix la trucada.</p>
<p>Si teniu <a href="/blog/2005/04/21/asterisk-the-open-source-linux-pbx/">instal·lat l'Asterisk</a>, per a enllaçar-lo amb VoipJet s'ha de fer la següent configuració:</p>
<p>1) Al fitxer <code>iax.conf</code>:</p>
<pre>
[voipjet]
type=peer
host= 216.118.117.46
secret= secret_obtingut_a_voipjet
auth=md5
notransfer=yes
context=default
</pre>
<p>2) Aquest pas és opcional. A la secció <code>codec</code> del fitxer <code>iax.conf</code> configurem el codec a <code>G.711 ulaw</code> per a qualitat óptima de sò i minim <em>delay</em> en la transmissió.</p>
<pre>
disallow=all ; Prevent all codecs...
allow = ulaw ; ...except G.711 ulaw
</pre>
<p>3) També opcionalment, just després de la secció <code>codec</code> del fitxer <code>iax.conf</code> habilitem el buffer de jitter:</p>
<pre>
jitterbuffer=yes ; Jitter buffer enabled...
dropcount=1 ; ...to drop at most 0.5% of VoIP packets
</pre>
<p>4) Al fitxer <code>extensions.conf</code>, afegir el següent, modificant 0000 pel nostre userid de VoipJet i canviant els deu ceros del CallerID pel número que volem que aparegui quan fem les trucades (si, ho has entes bé, et permet posar el numero que vulguis al CallerID!!).</p>
<pre>
; NANPA: North American Numbers dialed as 1 + area code
; For example, the New York Public Library is dialed as 12123400849
; 1 (North American call) 212 (New York area code) 3400849 (libary's phone number)
; WORLD: International Numbers dialed as 011 + country code + number
; For example, the Tate Modern Museum in London, U.K. is dialed as 011442078878000
; 011 (International call) 44 (U.K. country code) 2078878000 (museum's number)
; Finally, the number just before @voipjet in the Dial string is your VoipJet userid #

exten =&gt; _1NXXNXXXXXX,1,SetCallerID(0000000000); Set your CallerID as a ten digit number like this. See our FAQ
exten =&gt; _1NXXNXXXXXX,2,Dial,IAX2/0000@voipjet/${EXTEN} ; VoipJet.com NANPA
exten =&gt; _011.,1,SetCallerID(0000000000); Set your CallerID as a ten digit number like this. See our FAQ.
exten =&gt; _011.,2,Dial,IAX2/0000@voipjet/${EXTEN} ; VoipJet.com WORLD
;Do not change IAX2/0000 in the above two lines!
</pre>
<p>Per fer la trucada només haurem de connectar al asterisk ---amb <a href="http://www.wirlab.net/kphone/">kphone</a> per exemple---
i marcar el número amb `011+codi_pais+numero_telf`.

Ara penseu en les possibilitats... si connectem l'asterisk amb la linea telefònica de casa amb una tarja de [Digium](http://www.digium.com/), i contractem un servei de l'estil [mi preferido](http://www.amena.com/amena/particulares/tarjeta_amena/4_5.html) d'amena que permet fer trucades a un número a 3 centims per minut a qualsevol hora... podem trucar a Estats Units per 4 centims d'euro el minut des-de el movil en qualsevol moment ;)

