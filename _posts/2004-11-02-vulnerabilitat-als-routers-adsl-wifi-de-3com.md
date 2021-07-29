---
layout: post
title: Vulnerabilitat als routers ADSL WiFi de 3Com
date: 2004-11-02 12:38:53.000000000 +01:00
type: post
categories:
- security
- wireless
tags: []
meta:
permalink: "/2004/11/02/vulnerabilitat-als-routers-adsl-wifi-de-3com/"
---
Els routers ADSL WiFi 3CRADSL72 de 3Com, que regalen moltes telcos espanyoles al donar d'alta l'ADSL WiFi contenen una vulnerabilitat que encara no està solucionada per cap versió de firmware. Només cal accedir a la url `http://ip_del_router/app_sta.stm` per obtenir privilegis d'administrador sense necessitat d'autenticar-se.

Més informació [a SecurityFocus](http://www.securityfocus.com/bid/11408/exploit/) i a [PacketStorm](http://packetstormsecurity.org/0410-advisories/3comRouter.txt).

Els models 3CRWE754G72-A també tenen dues vulnerabilitats, que [si han estat solucionades pel fabricant](http://www.3com.com/products/en_US/result.jsp?selected=6&sort=effdt&sku=3CRWE754G72-A&order=desc). Més informació a PacketStorm: [Information disclosure](http://packetstormsecurity.org/0410-advisories/3com3crwe754g72-a.txt) i [Administration interface code injection](http://packetstormsecurity.org/0410-advisories/3com3crwe754g72-a2.txt).

