---
layout: post
title: Esnifant converses VoIP
date: 2005-10-16 21:42:11.000000000 +02:00
type: post
categories:
- linux
- security
- voip
tags: []
meta:
  tags: voipong vomit voip sniff
permalink: "/2005/10/16/esnifant-converses-voip/"
---
[VoIPong](http://www.enderunix.org/voipong/) és una aplicació que permet detectar trucades VoIP, soporta els protocols SIP, H323, Cisco's Skinny Client Protocol, RTP i RTCP, i si les trucades estan codificades amb G711 permet capturar-les en fitxers WAV.

Aquí teniu un exemple, capturant una trucada feta amb l'[SJPhone](http://www.sjlabs.com/sjp.html) usant [Asterisk](http://www.asterisk.org/):

```
# voipong -c /etc/voipong.conf -d 4 -f EnderUNIX VOIPONG Voice Over IP Sniffer starting... Release 1.2-DEVEL, running on cool [Linux 2.6.9-ac12 #1 Thu Dec 9 23:47:02 CET 2004 i686] (c) Murat Balaban http://www.enderunix.org/ 16/10/05 20:45:02: EnderUNIX VOIPONG Voice Over IP Sniffer starting... 16/10/05 20:45:02: Release 1.2-DEVEL running on cool [Linux 2.6.9-ac12 #1 Thu Dec 9 23:47:02 CET 2004 i686]. (c) Murat Balaban http://www.enderunix.org/ [pid: 8073] 16/10/05 20:45:02: Default matching algorithm: lfp 16/10/05 20:45:02: loadnet(192.168.1.0/255.255.255.0) method: lfp 16/10/05 20:45:02: loadnet(192.168.1.101/255.255.255.255) method: fixed 49164 16/10/05 20:45:02: loadnet(192.168.1.1/255.255.255.255) method: fixed 13078 16/10/05 20:45:02: ath0 has been opened in promisc mode. (192.168.1.0/255.255.255.0) 16/10/05 20:45:02: [8074] VoIP call has been detected. 16/10/05 20:45:02: [8074] 192.168.1.1:13078 192.168.1.101:49164 16/10/05 20:45:02: [8074] Encoding: 0-PCMU-8KHz 16/10/05 20:45:34: [8074] maximum waiting time [10 secs] has been elapsed for this call, the call might have been ended. 16/10/05 20:45:34: .WAV file [output/20051016/session-enc0-PCMU-8KHz-192.168.1.1,13078-192.168.1.101,49164.wav] has been created successfully. 16/10/05 20:45:34: .WAV file [output/20051016/session-enc0-PCMU-8KHz-192.168.1.101,49164-192.168.1.1,13078.wav] has been created successfully.
```

Una altra aplicació interessant és [Vomit](http://vomit.xtdnet.nl/), que convereix una captura en format pcap (tcpdump o ethereal) d'una trucada feta amb un telèfon IP Cisco (G711) en un fitxer WAV.

