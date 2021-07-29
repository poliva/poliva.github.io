---
layout: post
title: Internet (GPRS / 3G), MMS i Vodafone live! en WM5
date: 2006-08-20 00:33:14.000000000 +02:00
type: post
categories:
- gadgets
- wireless
tags: []
meta:
  tags: vodafone mms gprs 3g internet settings vodafone+live wm5
permalink: "/2006/08/20/internet-gprs-3g-mms-i-vodafone-live-en-wm5/"
---
Si heu llegit el [post anterior](/blog/2006/08/16/htc-hermes-wizard-blueangel-i-altres-histories-per-a-no-dormir/) sabreu que m'he pasat a contracte amb Vodafone, així que a continuació explico com configurar el windows mobile 5 per a conectar a internet amb 3G o GPRS, configurar els MMS i accedir a Vodafone Live! comento una mica els preus que té actualment cada cosa i dono algun consell per als que heu contactat la promoció [destinació vodafone](http://www.destinovodafone.com).

<!--more-->

### Internet 3G / GPRS

Desde el menu d'inici, seleccionem _Settings_ i anem a la pestanya _Connections_, un cop aquí fem click a la icona _Connections_ i ens apareixerà una pantalla com a aquesta:

![VF]({{ site.baseurl }}/assets/images/2006/08/vf001.png)

La diferència entre _My ISP_ i _My work network_ és que a _My ISP_ no podem configurar un servidor proxy i a _My work network_ si, per tant, com per aquesta connexió no necessitem cap proxy seleccionem la opció _Add a new modem connection_ sota de _My ISP_. A continuació posem un nom per a la conexió, per exemple "vodafone" i sel·leccionem _Cellular Line (GPRS, 3G)_ com a modem, fem click a _Next_ i introduim el <acronym title="Access Point Name">APN</acronym> "airtelnet.es".

![VF]({{ site.baseurl }}/assets/images/2006/08/vf002.png) ![VF]({{ site.baseurl }}/assets/images/2006/08/vf003.png)

El Nom d'usuari i la contrasenya es VODAFONE i VODAFONE, abans d'acabar fem click a _Advanced_ i marquem les opcions de _Use software compression_ i _Use IP header compression_, pulsem OK i ja hem acabat de configurar la connexió.

![VF]({{ site.baseurl }}/assets/images/2006/08/vf004.png) ![VF]({{ site.baseurl }}/assets/images/2006/08/vf005.png)

Quan connectem amb aquesta connexió obtenim una IP pública directament, sense passar per cap proxy. La tarifa de dades va en funció del [plà de dades](http://www.vodafone.es/Vodafone/ParticularesPS/ParticularesPS/0,2603,21621,00.html) que tinguem contractat, amb contracte sense contractar cap pla de dades el preu és de 0,5€/sessió, i cada "sessió" per a Vodafone son 256 Kb o 10minuts de connexió, es a dir, 2€/Mb si fem tràfic o 3€/hora si no fem tràfic.

Nota: Aquesta connexió no ve configurada als mòvils brandejats per Vodafone, la connexió que tenen preconfigurada és la de VF live que comentaré al final, per tant, per a la gent que t'atén al <acronym title="Servei d'Atenció Telefònica">SAT</acronym> això és com si et connectessis amb un PC utilitzant el mòvil com a modem.

### MMS

En aquest cas, com tampoc necessitem cap proxy afegirem una nova connexió sota _My ISP_ al apartat _Connections_ que hem comentat anteriorment. Li donem un nom a la nova connexió, per exemple "Vodafone MMSC" i sel·leccionem _Cellular Line (GPRS, 3G)_ com a modem, fem click a _Next_ i introduim el <acronym title="Access Point Name">APN</acronym> "mms.vodafone.net".

![VF]({{ site.baseurl }}/assets/images/2006/08/vfmms001.png) ![VF]({{ site.baseurl }}/assets/images/2006/08/vfmms002.png)

El Nom d'usuari és "wap@wap" i la contrasenya és "wap125", abans d'acabar fem click a _Advanced_ i marquem les opcions de _Use software compression_ i _Use IP header compression_, pulsem OK i ja hem acabat de configurar la connexió, però encara hem de configurar el MMS composer per a que utilitzi aquesta connexió.

![VF]({{ site.baseurl }}/assets/images/2006/08/vfmms003.png) ![VF]({{ site.baseurl }}/assets/images/2006/08/vf005.png)

Accedim al correu a través del menú d'inici, fem click a _Messaging_, desde el Menu, seleccionem _Tools -\> Options_, i clickem a sobre de MMS per accedir a la configuració. A la pestanya de _Preferences_, jo tinc marcades les opcións que veieu a continuació, però això va al gust de cadascú:

![VF]({{ site.baseurl }}/assets/images/2006/08/vfmms004.png)

Seguidament fem click a _Servers_ i n'afegim un de nou fent click al botó _New_. Al _server name_ posem per exemple "vodafone mms", el gateway és 212.73.32.10, el port 9201 i l'adreça <tt>http://mmsc.vodafone.es/servlets/mms</tt>. Com la connexió la hem configurat a _My ISP_, això és el que hem de triar al desplegable de _Connect via_ i finalment posem un limit de _100K_ per MMS (o més si voleu) i triem la versió _WAP 1.2_, per finalitzar feu click a _ok_:

![VF]({{ site.baseurl }}/assets/images/2006/08/vfmms005.png) ![VF]({{ site.baseurl }}/assets/images/2006/08/vfmms006.png)

Per acabar, ens hem d'assegurar de que el servidor que hem afegit queda com a servidor per defecte, ho veiem perqué apareix una fletxa de color vermell al costat. Rebre MMS és gràtis, i [aquí podeu consultar els preus](http://www.vodafone.es/Vodafone/EmpresasPS/Terminales/TerminalesFicha_Vodafone/0,,23125,00.html) d'enviament, que bàsicament són 0,2€ si ocupa menys de 1K, 0,6€ de 1 a 30K i 1,5€ de 30 a 300Kbytes.

### Vodafone live!

Per acabar anem a configurar el servei Vodafone Live!, s'ha d'anar en compte perque això és el timo de la estampeta de vodafone, sense contractar res especial ens diuen que tenim una _Tarifa Plana Live! Por Conexión_ això vol dir que paguem sempre 0,5€ per cada connexió que fem a VF live i rés més si no sortim de la pàgina de VF live, però la **navegació fora de VF live** surt 1€/Mb tarificat en blocks de 512K (400k = 0,5€ i 600k = 1€). El truc està en que dins de la pàgina de VF live està plé d'enllaços a contingut extern, i normalment desde el navegador d'un mòvil no és molt fàcil veure on apunta la URL d'un enllaç sense fer click a l'enllaç, així que ja us he avisat!

Amb aquesta connexió ens donen una IP privada i podem sortir a internet **només per HTTP** però sempre detràs d'un proxy.

En aquest cas, si que necessitem utilitzar un servidor proxy, per tant afegirem la nova connexió sota _My work network_ al apartat _Connections_ que hem comentat anteriorment. Li donem un nom a la nova connexió, per exemple "Vodafone Live!" i sel·leccionem _Cellular Line (GPRS, 3G)_ com a modem, fem click a _Next_ i introduim el <acronym title="Access Point Name">APN</acronym> "airtelwap.es".

![VF]({{ site.baseurl }}/assets/images/2006/08/vflive001.png) ![VF]({{ site.baseurl }}/assets/images/2006/08/vflive002.png)

El Nom d'usuari és "wap@wap" i la contrasenya és "wap125", abans d'acabar fem click a _Advanced_ i marquem les opcions de _Use software compression_ i _Use IP header compression_, pulsem OK i ja hem acabat de configurar la connexió, però encara hem de configurar el proxy.

![VF]({{ site.baseurl }}/assets/images/2006/08/vflive003.png) ![VF]({{ site.baseurl }}/assets/images/2006/08/vf005.png)

Accedim a la pestanya _Proxy Settings_ i sel·leccionem les opcions _This network connects to the Internet_ i _This network uses a proxy server to connect to the Internet_. L'adreça del proxy és 212.73.32.10. Seguidament fem click a _Advanced_ i configurem el proxy per HTTP (port 80), WAP (port 9201) i socks (port 1080).

![VF]({{ site.baseurl }}/assets/images/2006/08/vflive004.png) ![VF]({{ site.baseurl }}/assets/images/2006/08/vflive005.png)

Per accedir a la pàgina de VF live heu d'entrar a la URL **wsp://live.vodafone.com** (també es pot per http://, però el wsp:// fà que el windows connecti per WAP utilitzant _My work network_ i així no heu de connectar a mà). I això és tot, podeu consultar les [tarifes de vodafone live! aquí](http://www.vodafone.es/Vodafone/ParticularesPS/ParticularesPS/0,2603,23068,00.html).

### Notes sobre la promoció Destino Vodafone

Per als que tingueu la promoció _Destino Vodafone_, vàlida fins al 21 de Setembre de 2006, la lletra petita diu això:

_"INTERNET: connexió gratuïta a Vodafone live! i a Internet fins a 250Mb/mes. A partir de 250Mb, segons Pla de Preus. Exclosa navegació fora de Vodafone live!"_

En paraules clares això vol dir que no us estan cobran els 0,5€/connexió a VF live, però si que us cobren el tràfic HTTP que feu si navegeu (fora de VF Live) a través de la connexió VF live, per tant **no utilitzeu aquesta connexió per navegar**. Amb la connexió de GPRS / 3G (la primera d'aquest post) hi ha 250Mb/mes gratuïts i sortiu directament a internet sense proxy, per tant aquesta és la que s'ha d'utilitzar.

Una altra cosa a tenir en compte és que es **impossible** saber quin consum teniu, ja que al fer una consulta de consum, ja sigue desde el movil (\*131#) o desde internet a través de [Mi vodafone](https://mivodafone.vodafone.es/) les promocions i els descomptes no estàn aplicats.

