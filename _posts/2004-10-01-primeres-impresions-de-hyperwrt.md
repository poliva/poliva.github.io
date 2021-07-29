---
layout: post
title: Primeres impresions de HyperWRT 1.4
date: 2004-10-01 13:48:44.000000000 +02:00
type: post
categories:
- gadgets
- linux
- wireless
tags: []
meta:
permalink: "/2004/10/01/primeres-impresions-de-hyperwrt/"
---
Ahir per la nit vaig estar jugant amb el meu Linksys WRT54G, i vaig provar la versió 1.4 de [HyperWRT](http://www.hyperdrive.be/hyperwrt/), fins ara el millor firmware que he vist per a aquest dispositiu.

La filosofia d'aquest firmware és proporcionar el firmware més proper possible a la última versió del firmware original de LinkSys, amb petites modificacions, molt minimalistes que el fan molt, però que molt potent: Navegant pels menus de configuració es poden veure les [característiques](http://www.hyperdrive.be/hyperwrt/index.php?page=features) que afegeix HyperWRT i que no s'obtenen a través del firmware origina, que són les següents:

· Potencia de transmissió ajustable de 0 a 84mW  
· Possibilitat de seleccionar l'antena (dreta, esquerra i auto) per a RX i TX  
· 13 canals disponibles  
· Protecció 'Boot Wait': Quan s'activa el "Boot Wait" el router espera 5 segons quan reinicia amb un servidor TFT escoltant als ports Ethernet, aixó permet que si al flashejar un firmware nou 'la caguem' podem poguem tornar a carregar-li el firmware correcte a través de TFT mentre es reinicia utilitzant un client TFTP.  
· Accés shell a través del interface web  
· Scripts configurables de startup i firewall  
· Possibilitat de consultar uptime i cpuload via web  
· Botó de reboot, via web  
· Fa visibles les noves característiques ja disponibles al firmware de linksys, però que no son accessibles via web al firmware original.  
· I finalment, la _joia de la corona_, **suport de Addons** :

El firmware deixa uns 4300 kB de ram disponible per a carregar aplicacions un cop el router ha arrancat, aixó ens permet poder descarregar els nostres pròpis paquets amb _wget_ i carregar-li aplicacions sense haver de flashejar-lo de nou. Evidentment, aquestes aplicacions que carreguem en ram desapareixen quan rebotem el router, però podem allotjar un tar amb les nostres modificacions en un servidor web i fer un script de startup que es descarregui el tar i el descomprimeixi per tornar a tenir les aplicacions después de rebootar.

Amb aixó podem fer-nos el firmware 'a mida', ens dona tota la potència per carregar-li les aplicacions que vulguem sense por de fer malbé el router, ja que tot es carrega en ram.

Per compilar les aplicacions per al Linksys, necessitarem un _<acronym title="compilador creuat">cross compiler</acronym>_ que ens permeti compilar per a arquitectura MIPS, però si no volem cap aplicació molt específica, podem **aprofitar els [paquets de OpenWRT](http://openwrt.org/ipkg/)!** Per fer-ho només haurem de descomprimir el paquet _ipkg_ com si fos un .tar.gz i extreure els fitxers que necessitem del arxiu data.tar.gz que trovarem dins del _ipkg_. Jo ja li he carregat unes quantes coses, ara estic [lluitant per que em funcioni](http://www.linksysinfo.org/modules.php?name=Forums&file=viewtopic&t=1032) el [chillispot](/blog/2004/09/10/66/).

