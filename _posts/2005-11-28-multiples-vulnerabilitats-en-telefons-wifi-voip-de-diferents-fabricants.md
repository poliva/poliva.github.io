---
layout: post
title: Múltiples vulnerabilitats en telèfons WiFi VoIP de diferents fabricants
date: 2005-11-28 18:35:19.000000000 +01:00
type: post
categories:
- security
- voip
- wireless
tags: []
meta:
  tags: wifi voip handset security vulnerability
permalink: "/2005/11/28/multiples-vulnerabilitats-en-telefons-wifi-voip-de-diferents-fabricants/"
---
Durant el passat [<acronym title="Computer Security Institute">CSI</acronym> Computer Security Conference & Exhibition](http://www.csi32nd.com/), Shawn Merdinger va presentar una sessió titulada [VoIP WiFi Phone Handset Security Analysis](https://www.cmpevents.com/CSI32/a.asp?option=G&V=3&id=406438) on va fer públiques diverses vulnerabilitats en telèfons VoIP Wifi de múltiples fabricants, aquí teniu els models afectats i les vulnerabilitats publicades:

- Producte: [Senao SI-680H VOIP WIFI Phone](http://www.senao.com/english/product/product_wired_dsl_1.asp?tp1id=03&tp2id=02&proid=000186)
  1. Vulnerabilitat: [undocumented open port UDP/17185](http://packetstormsecurity.org/0511-advisories/SenaoVOIP.txt)
- Producte:[Zyxel P2000W Version 1 VOIP WIFI Phone](http://www.zyxel.com/product/P2000W.php)
  1. Vulnerabilitat: [undocumented port UDP/9090](http://packetstormsecurity.org/0511-advisories/ZyxelVOIP.txt)
  2. Vulnerabilitat: [Phone uses hardcoded DNS servers](http://packetstormsecurity.org/0511-advisories/ZyxelVOIP.txt)
- Producte: [UTStarcom F1000 VOIP WIFI Phone](http://www.utstar.com/Solutions/Handsets/WiFi/)
  1. Vulnerabilitat: [SNMP daemon has default public read credentials and the daemon cannot be disabled](http://packetstormsecurity.org/0511-advisories/UTStarcomVOIP.txt)
  2. Vulnerabilitat: [telnet server has known default user/password credentials (l:target / p:password)](http://packetstormsecurity.org/0511-advisories/UTStarcomVOIP.txt)
  3. Vulnerabilitat: [rlogin (TCP/513) unauthenticated access](http://packetstormsecurity.org/0511-advisories/UTStarcomVOIP.txt)
- Producte: [Hitachi IP5000 VOIP WIFI Phone](http://www.wirelessip5000.com/)
  1. Vulnerabilitat: [handset hardcoded administrator password (0000)](http://packetstormsecurity.org/0511-advisories/hitachiVOIP.txt)
  2. Vulnerabilitat: [HTTP server vulnerabilities](http://packetstormsecurity.org/0511-advisories/hitachiVOIP.txt) (Improper information disclosure & Web server default configuration does not require credentials to  
authenticate)
  3. Vulnerabilitat: [SNMP daemon vulnerabilities](http://packetstormsecurity.org/0511-advisories/hitachiVOIP.txt)
  4. Vulnerabilitat: [undocumented port TCP/3390 Unidata Shell](http://packetstormsecurity.org/0511-advisories/hitachiVOIP.txt)
- Producte: [Cisco 7920 Wireless IP Phone](http://www.cisco.com/en/US/products/hw/phones/ps379/products_data_sheet09186a00801739bb.html)
  1. Vulnerabilitat: [Fixed SNMP Community Strings](http://packetstormsecurity.org/0511-advisories/cisco-sa-20051116-7920.txt)
  2. Vulnerabilitat: [VxWorks Debugger Port (wdbrpc, 17185/udp)](http://packetstormsecurity.org/0511-advisories/cisco-sa-20051116-7920.txt)
