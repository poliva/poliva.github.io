---
layout: post
title: Detalls sobre la connexió desde l'avió
date: 2005-07-15 03:45:10.000000000 +02:00
type: post
categories:
- personal
- wireless
tags: []
meta:
  tags: ''
permalink: "/2005/07/15/detalls-sobre-la-connexio-desde-lavio/"
---
<p>Aprofitant el vol de tornada he tornat a pagar els casi $30 que val connectar a internet desde l'avió durant tot el vol, amb el servei <a href="http://portal.lufthansa.com/online/portal/lh/de/info_services/mobile_services?l=en&amp;nodeid=1144212">FlyNet</a> de <a>Lufthansa</a>, ofert per <a href="http://www.connexionbyboeing.com/">Connexion by Boeing</a>. Aquest cop no m'ha tocat passar pel procés de registre, ja que ja tinc un compte creat del viatge d'anada, el sistema també se'n recordava de les meves dades i no m'ha calgut tornar a posar la visa (al procés de registre li vaig dir que la recordés).</p>
<p>Ara porto aproximadament 3 hores connectat, i durant aquesta estona la connexió s'ha tallat 2 cops, el primer durant uns 5 minuts i el segón menys de 2 minuts. Suposo que aquests talls es deuen a que l'antena que porta l'avió no pot orientar-se correctament cap al satel·lit, ja que ha d'anar rotant al llarg del trajecte. La distribució d'internet dintre de l'avió és fa a través de 4 APs amb SSID <em>Connexion1</em> que donen cobertura de sobres a tots els seients, tant en Business Class com en Turista. També hi ha una xarxa adhoc, amb SSID <em>NETGEAR</em> que no he tingut temps d'indagar, potser si després em queda batería faig alguna cosa per descobrir que és... al viatge d'anada també hi era, pero vaig pensar que sería algun portatil d'algún altre passatger mal configurat... es veu que no, ja que aquest cop l'he tornat a trobar.</p>
<p>Al connectar a la xarxa wifi, obtens la IP per DHCP, del rang 172.16.64/19.  Es a dir, estem darrera d'un NAT. Connectant a casa per HTTP als logs del servidor web apareix la IP 216.65.237.216, en canvi connectant per HTTPS apareix la IP 83.210.35.2, que també es la que surt quan faig un who si connecto per SSH. Es a dir, que tinc una ip d'origen diferent en funció del protocol al que accedeixo (utilitzen enrutament per port).</p>
<p>La connexió desde l'avió va bastant ràpida, només arribar he baixat el firefox (8,5Mb) a una mitja de 45Kbytes/s, ara estic baixant un tarball de 36Mb de kernel.org a 56Kbytes/s de mitja.</p>
<p>Segons el <a href="www.dslreports.com/stest">test de velocitat</a> de DSL Reports:</p>
<pre>
2005-07-14 21:48:08 EST: 294 / 26
Your download speed : 301361 bps, or 294 kbps.
A 36.7 KB/sec transfer rate.
Your upload speed : 26724 bps, or 26 kbps.
</pre>
<p>Traceroute a google:</p>
<pre>
$ traceroute www.google.com
traceroute: Warning: www.google.com has multiple addresses; using 66.102.7.147
traceroute to www.l.google.com (66.102.7.147), 64 hops max, 40 byte packets
 1  172.16.64.1 (172.16.64.1)  2.963 ms  3.759 ms  2.177 ms
 2  cbb-cds-psn.by.boeing (172.16.0.18)  2.042 ms  2.052 ms  1.984 ms
 3  sbs.by.boeing (172.31.0.1)  3.252 ms  2.151 ms  2.207 ms
 4  * * *
 5  10.8.20.35 (10.8.20.35)  576.390 ms  607.023 ms  579.966 ms
 6  ltn02r03-vlan25.connexionbyboeing.net (10.8.20.2)  581.988 ms  581.138 ms  583.917 ms
 7  ltn02r21-fa2-9.connexionbyboeing.net (10.8.16.25)  953.058 ms  579.085 ms  584.297 ms
 8  10.8.16.33 (10.8.16.33)  579.090 ms  609.747 ms  579.793 ms
 9  ltn02r01-fa3-3.connexionbyboeing.net (10.8.16.130)  582.170 ms  581.743 ms  706.597 ms
10  border10.s6-2.boeing-2.den.pnap.net (63.251.181.181)  583.828 ms  574.777 ms  579.702 ms
11  core3.ge2-0-bbnet1.den.pnap.net (216.52.40.3)  582.148 ms  607.487 ms  579.421 ms
12  so-4-1.hsa2.denver1.level3.net (64.156.40.65)  582.125 ms  594.274 ms  623.331 ms
13  so-6-0-0.bbr1.denver1.level3.net (4.68.113.49)  751.455 ms  604.446 ms  585.222 ms
14  ae-0-0.bbr1.sanjose1.level3.net (64.159.1.129)  613.589 ms as-1-0.bbr2.sanjose1.level3.net (64.159.0.242)  634.088 ms  694.210 ms
15  ge-7-1.ipcolo1.sanjose1.level3.net (4.68.123.73)  612.396 ms ge-11-0.ipcolo1.sanjose1.level3.net (4.68.123.41)  642.618 ms ge-7-0.ipcolo1.sanjose1.level3.net (4.68.123.9)  640.276 ms
16  4.79.216.58 (4.79.216.58)  648.491 ms  645.379 ms  624.975 ms
17  66.249.94.29 (66.249.94.29)  620.118 ms  621.601 ms  623.910 ms
18  216.239.49.154 (216.239.49.154)  625.154 ms 216.239.49.150 (216.239.49.150)  652.132 ms 216.239.49.154 (216.239.49.154)  633.548 ms
19  * * *
</pre>
<p>Ping a google:</p>
<pre>
$ ping -c 10 www.google.com
PING www.l.google.com (66.102.7.147): 56 data bytes
64 bytes from 66.102.7.147: icmp_seq=0 ttl=233 time=590.299 ms
64 bytes from 66.102.7.147: icmp_seq=1 ttl=233 time=605.974 ms
64 bytes from 66.102.7.147: icmp_seq=2 ttl=233 time=597.027 ms
64 bytes from 66.102.7.147: icmp_seq=3 ttl=233 time=596.249 ms
64 bytes from 66.102.7.147: icmp_seq=4 ttl=233 time=620.012 ms
64 bytes from 66.102.7.147: icmp_seq=5 ttl=233 time=616.620 ms
64 bytes from 66.102.7.147: icmp_seq=6 ttl=233 time=614.984 ms
64 bytes from 66.102.7.147: icmp_seq=7 ttl=233 time=611.453 ms
64 bytes from 66.102.7.147: icmp_seq=8 ttl=233 time=607.699 ms
64 bytes from 66.102.7.147: icmp_seq=9 ttl=233 time=608.169 ms

--- www.l.google.com ping statistics ---
10 packets transmitted, 10 packets received, 0% packet loss round-trip min/avg/max/stddev = 590.299/606.849/620.012/9.176 ms

Si després segueixo indagant, ja faré un update... ara vaig a seguir llegint [bloglines](http://www.bloglines.com), que tinc 8000 posts sense llegir!!! ;)

