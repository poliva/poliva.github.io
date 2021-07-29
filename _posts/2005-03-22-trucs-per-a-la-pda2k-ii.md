---
layout: post
title: Trucs per a la PDA2k (II)
date: 2005-03-22 02:21:35.000000000 +01:00
type: post
categories:
- gadgets
- HTC
tags: []
meta:
permalink: "/2005/03/22/trucs-per-a-la-pda2k-ii/"
---
Continuant amb els [trucs per a la PDA2k](/blog/2004/11/26/trucs-per-a-la-pda2k/), recopilo els últims que he aprés:

**Activar el _notify_ per defecte als SMS nous** [[via](http://wiki.xda-developers.com/index.php?pagename=BA_Hacks)]  
1) Amb un editor de registre, accedir a la clau `\HKCU\Software\Microsoft\Inbox\Settings\`.  
2) Afegir la Dword "`SMSDeliveryNotify`" si no existeix.  
3) Donar-li el valor "`(0x000001)`" o "`1`" en funció de quin editor de registre utilitzem.  
Per comprovar que ha funcionat crear un nou SMS i mirar `Tools > Options`; la opció _delivery notification_ hauria d'aparèixer marcada per defecte.  
NOTA: Només funciona per a missatges nous, s'ha d'activar el notify manualment si es fa _reply_ o _forward_.

**Sol·lució "definitiva" per al problema del bluetooth wake-up** [[via](http://forum.xda-developers.com/viewtopic.php?t=17456)]  
Només cal instal·lar aquest patch: [WLANPatch.CAB](ftp://xda:xda@ftp.xda-developers.com/BlueAngel/Extended_ROM_Kitchen_1_3X/Cabs/Patch/WLANPatch.CAB)

**Cuinar la extended rom**  
La _extended rom_ de la PDA és una part de memòria amagada, on estan els fitxers CAB dels programes que s'instal·len després d'un hard reset. En el cas de la PDA2k son els programes afegits per i-mate com per exemple el software de la camara, el midlet manager o el d'enviar FAX. Es possible fer visible aquesta part de la ROM, i modificar-la per afegir el software que nosaltres volem que s'instal·le per defecte després d'un hard reset. Això és el que es diu una _cooked rom_ o rom cuinada. A xda-developers hi ha la _Extended Rom Kitchen_ que és una rom que recopila software i parches que inclouen els distribuïdors i diferents operadors en les seues extended roms. Més informació [aquí](http://forum.xda-developers.com/viewtopic.php?t=16137).

**Canviar la boot image**  
Sol·lució per a [canviar la boot image](http://forum.xda-developers.com/viewtopic.php?t=12898) si no t'agrada el logo de i-mate.

**Upgrade del software de la camara**  
[Versió 2.21 (Build 17576)](ftp://xda:xda@ftp.xda-developers.com/BlueAngel/Extended_ROM_Kitchen_1_22/Cabs/Patch/Camera_Patch_10080401.sa.CAB)

**IntelliDial i IntelliPad**  
Aquests dos programes els han extret de la ROM de la i-mate JAM, i jo els he trobat molt útils. El IntelliDial es un _smart dialer_ que permet buscar a l'agenda només polsant el teclat numèric del telèfon.  
IntelliPad és una extensió per al teclat per software, que permet escriure amb els dits, jo l'he trobat molt útil per als SMS, ja que d'aquesta manera només cal utilitzar una sola mà. També suporta T9.  
Download: [IntelliDial](ftp://xda:xda@ftp.xda-developers.com/BlueAngel/Various%20CAB%20upgrades%20and%20progs/SmartDialler/SmartDialing_WWE_RC11.CAB) i [IntelliPad](ftp://xda:xda@ftp.xda-developers.com/BlueAngel/Various%20CAB%20upgrades%20and%20progs/Intellipad/IntelliPad_WWE_B10.CAB).

**bass & treble**  
Programa que permet ajustar els greus i aguts del só de la PDA.  
[Download](ftp://xda:xda@ftp.xda-developers.com/BlueAngel/Various%20CAB%20upgrades%20and%20progs/bass&treble/dsp_en.cab).

**ClearVue**  
Aquest software permet veure arxius PPT (power point) i PDF. [ClearVue2.4.386](ftp://xda:xda@ftp.xda-developers.com/BlueAngel/CAB_Files/ClearVue/ClearVue2.4.386_WWE.CAB).

**Activar el mans lliures integrat**  
Només cal mantenir polsat el botó de trucada (el verd) durant dos segons mentre s'està fent una trucada.

