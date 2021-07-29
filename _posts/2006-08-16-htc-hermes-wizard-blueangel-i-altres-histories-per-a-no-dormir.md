---
layout: post
title: HTC Hermes, Wizard, BlueAngel i altres històries per a no dormir
date: 2006-08-16 23:36:18.000000000 +02:00
type: post
categories:
- gadgets
- HTC
- linux
- personal
tags: []
meta:
  tags: htc tytn hermes wizard blueangel linux pocketpc vodafone 3g
permalink: "/2006/08/16/htc-hermes-wizard-blueangel-i-altres-histories-per-a-no-dormir/"
---
Desde que m'he comprat la HTC TyTN porto uns quants dies bastant "away", tant del blog com del mon real, però no he estat parat... volia fer varios posts amb totes les coses que he anat fent però no tinc prou temps, així que faig un post amb un resum de tot i ja aniré fent posts més detallats quan tingui temps.

### HTC TyTN

És una passada, va arribar 3 dies després de demanar-la a expansys amb aquesta rom:

`
ROM Version: 1.18.255.3
ROM Date: 05/30/06
Radio version: 1.03.03.10
Protocol version: 32.34.7010.01H
ExtROM version: 1.18.255.105
CE version: OS 5.1.195 (Build 14955.2.3.0)
Model No.: HERM200
`

No faré un review extensiu perqué a aquestes altures ja en podeu trobar uns quants per internet, si algú té dubtes sobre alguna cosa només cal que pregunti i si puc li contestaré, però en general estic molt més que content amb la Hermes i la recomano a tothom que vulgui un PocketPC phone ara mateix.

He estat bastant actiu al site de xda-developers, porto [140 posts en 11 dies](http://forum.xda-developers.com/search.php?search_keywords=&search_terms=any&search_author=pof&search_forum=-1&search_time=0&search_fields=all&search_cat=-1&sort_by=0&sort_dir=DESC&show_results=topics&return_chars=200) als forums i també he contribuit moltíssim a la [wiki de HTC Hermes](http://wiki.xda-developers.com/index.php?pagename=HTC_Hermes), aquí teniu links a algunes págines interessants:

- [registry hacks](http://wiki.xda-developers.com/index.php?pagename=Hermes_Registry), alguns hacks per al registre que permeten fer coses interessants
- [HardReset i bootloader](http://wiki.xda-developers.com/index.php?pagename=Hermes_Resets), combinacions de tecles per posar-la PDA en modo bootloader, o fer-li un hard reset
- [utilities and software](http://wiki.xda-developers.com/index.php?pagename=Hermes_Utils), utilitats chorres, de moment hi ha el client de Vodafone per a push email i una utilitat (SetHSDPA.exe) que està inclosa en la pròpia PDA, però alguns dispositius no la porten habilitada.
- [SIM Unlock](http://wiki.xda-developers.com/index.php?pagename=Hermes_SimUnlock), de moment només hi ha una companyia que ho fa per 20 GBP, intentarem trobar una sol·lució gratuïta, encara que la gran majoría de dispositius venen sense SIMLock de serie, de moment els únics amb simlock son els distribuïts per Orange (SPV M3100).
- [Linux port](http://wiki.xda-developers.com/index.php?pagename=Hermes_Linux), de moment ningú ha contestat als meus posts sobre aquest tema, però hi ha una versió de haret.exe adaptada al processador de la PDA y no hauría de ser molt dificil bootar un kernel... també he estat parlant amb alguns dels developers que han portat linux a la Universal, a la BlueAngel i a la Wizard a través del #htc-linux de freenode, si hi ha progrés en aquest tema ja us avisaré.
- [Hermes versions](http://wiki.xda-developers.com/index.php?pagename=Hermes_Versions), amb tots els noms que té la HTC Hermes, diferencies entre els models HERM100 i HERM200, diferents versions de rom, etc distribuïdes en cada model...
- [FAQs](http://wiki.xda-developers.com/index.php?pagename=Hermes_FAQs)
- [Hermes Previews](http://wiki.xda-developers.com/index.php?pagename=Hermes_Previews), alguns reviews que han anat apareixent per diferents llocs, benchmarks, etc...
- [Hermes Problems](http://wiki.xda-developers.com/index.php?pagename=Hermes_Problems), alguns dispositius tenen problemes amb el teclat i amb l'aliniament de la pantalla, per sort la meva no té cap d'aquests problemes
- [Roms originals distribuides pels fabricants](http://wiki.xda-developers.com/index.php?pagename=Hermes_Upgrades), de moment només n'hi ha dos, una de Dopod 9000 i una altra de HTC TyTN, aquesta última és molt semblant a la que jo tinc, només cambia mínimament la extended rom
- [Dumped roms](http://wiki.xda-developers.com/index.php?pagename=Hermes_DumpedRoms), roms extretes del dispositiu directament, vaig ser el primer en dumpejar la meva, i al cap d'uns dies un bobby.kashyap va seguir els meus passos i va pujar una rom dumpejada d'un Dopod 838 pro. He estat mirant les diferencies i la seva té algunes coses més noves. Encara no sabem com empaquetar una rom extreta per fer un upgrade, pel que sembla la Rom Upgrade Utility de HTC Hermes utilitza un format diferent que la resta de dispositius de HTC, el fitxer que conté la rom (ExtROM, OS, IPL/SPL, SplashScreen, GSM radio, etc...) té la extensió .nbh, normalment aquest fitxer té extensió .nbf i es pot extreure utilitzant TyphoonNbfTool v5, però aquesta utilitat no funciona amb el nou format, de totes maneres no crec que triguem molt en descubrir com funciona el nou format...
- [Passos per dumpejar la rom](http://wiki.xda-developers.com/index.php?pagename=Hermes_HowtoDumpRom), originalment escrit per _itsme_, l'autor de les [itsutils](http://www.xs4all.nl/~itsme/projects/xda/tools.html) per dumpejar la rom, vaig extendre l'artícle després d'aconseguir dumpejar la meva, per facilitar el procés a la resta dels mortals. El procés del wiki explica com fer-ho amb pdocread (opció -w, perqué la Hermes no té Disc-on-Chip) després es poden extreure els continguts de la rom dumpejada amb rdmsflsh.pl. També he aconseguit dumpejar la rom amb TestDump.exe desde la PDA, però encara no he aconseguit viewimgfs o rdmsflsh.pl puguin extreure'n els continguts, aquest mètode encara no l'he explicat al wiki, perqué no tinc clar si funciona o no, així que de moment és mes fiable fer-ho amb pdocread.
- [Com fer visible la extended rom i desbloquejar-la per poder-hi escriure](http://wiki.xda-developers.com/index.php?pagename=Hermes_Unhide_Extrom)
- [Versions de extended rom disponibles](http://wiki.xda-developers.com/index.php?pagename=Hermes_ExtendedRoms), que la gent ha anat pujant al FTP a partir del link anterior que explica com fer-la visible, de moment hi ha les extended roms de casi totes les versions de HTC hermes que hi ha al mercat...

### Vodafone, push email, 3G...

L'adaptador dual-sim que tenía abans amb la BlueAngel no funciona amb la Hermes així que vaig haver de cambiar de la combinació que tenía amb dues prepagos (Amena per veu i Movistar per dades), per un contracte amb Vodafone, amb portabilitat del número amena que és el que té tothom que em coneix a l'agenda. M'he apuntat a la promoció d'estiu destino vodafone, i tinc trucades i vidiotrucades gratis a vodafone, 200Mb d'internet, televisió, etc... gratis fins al 21 de setembre, evidentment cal llegir la lletra petita perqué tot està una mica "limitat", però bueno. Respecte a Vodafone els he triat per les tarifes de dades que tenen, tot i que les tarifes de veu no m'acaben de convencer (m'he pillat un contrato tardes). Els primers dies fent consultes de consum (\*131#) vaig veure que augmentava molt ràpidament el consum, pero resulta que el descompte de la promoció destino vodafone no està aplicat, així que la consulta de consum només serveix per donar-li 12 cèntims a VF perqué no pots saber quants diners portes consumits si t'has donat d'alta a la promoció... El cap de setmana vaig baixar a Vinaròs en tren, vaig quedar bastant al·lucinat de veure la cobertura 3G que hi ha, durant el viatge tret dels 10 minuts de túnels se'm va tallar 3 cops la conexió i el 95% del tems estava amb 3G i no amb GPRS. He fet varios tests de velocitat i normalment están entre 285 i 370 Kbps, segons la cobertura, així que sembla que no tinc el HSDPA activat i a atenció al client no saben ni lo que és (els has de parlar de 3'5G i del anuncí de "trabajaras el doble" per que t'entenguin, però aquest tema encara no el tinc resolt perqué em diuen que desde el movil no se puede i volen que em compri una PCMCIA amb una linia específica per dades, els haig de tornar a fotre canya a veure si aconsegueixo que m'activin el HSDPA. També vaig trucar a atenció al client per preguntar com funcionava lo del push email, pero resulta que et cobren una quota mensual, total que al final m'ho he montat jo amb quatre linies de procmail per reenviar al servidor de push email només el correu que m'interessa (filtrant llistes de correu i spam) i utilitzant el servidor d'exchange gratuït que et donen a [mail2web](http://live.mail2web.com/) amb suport de DirectPush. No em dona molt bon rollo que tota la meva inbox passi per mail2web, així que he de mirar alternatives a exchange que funcionen sobre linux i soportin push email ([DirectPush](http://msexchangeteam.com/archive/2006/04/03/424028.aspx)), per montar-me la infrastructura a casa, sí algú em pot recomanar algo estaré molt agraït, sinó quan descobreixi la sol·lució ja postejaré... sembla mentida que la gent pagui per tenir push email de vodafone :P També volia parlar sobre el tema de la TV, amb el contracte Vodafone m'han regalat un motorola V1075 (alias motorola musclo) desde el qual puc veure la TV amb el menú vodafone live, però si accedeixo amb el pocket IE de la PDA el link de TV no surt, s'ha de canviar el user agent al registre per a que s'identifique com el motorola musclo... els streams de TV son fitxers amb extensió .sdp, que s'envien a través de rtsp, per veure-ho amb la PDA es pot utilitzar el PVPlayer, però sincerament no val la pena si no és només per provar-ho i fer la gràcia...

### HTC Wizard

A un colega de Vinaròs que està vivint a Holanda li van donar una HTC Wizard al fer un contracte amb T-Mobile, resulta que la Wizard portava el WM5 en holandés i éll la volía passar a inglés. Li vaig passar instrucions per fer el CID-Unlock i flashejar-li una rom amb anglés però misteriosament el upgrade va fallar i es va quedar sense Wizard (no arrancaba el S.O.) així que me la va deixar i encara la tinc a casa, salvar-la va ser relativament fàcil, només va caldre posar-la en modo bootloader i ja es va deixar flashejar. Li vaig posar primer la última rom de T-Mobile UK (WWE) pero no m'agradava molt (esta plena de customitzacions de TMob i el tema es molt lleig) així que vaig estar buscant a vore quina era la rom mes actual, i vaig trobar una cooked rom que es diu [Mr.Clean 2.24a](ftp://xda:xda@ftp.xda-developers.com/Wizard/Roms/MrClean_RUU_WIZARD_224a_AKU23_WWE.zip), el WM5 está extret de la última ROM de T-Mobile (AKU 2.3), y la Radio ROM està extreta de la última ROM de Cingular (2.25). Són les versions més actuals que existeixen.

Al bootar apareix aquesta informació:

`
IPL: 2.24
SPL: 2.24
GSM: 02.25.11
OS: 2.24.10.1
`

Dins del sistema operatiu:

`
ROM version: 2.24.10.1 WWE
ROM date: 4/17/06
Radio version: 02.25.11
Protocol version: 4.1.13.12
ExtROM version: 2.24a Mr. Clean
Windows version: OS 5.1.195 (Build 14955.2.3.0)
`

La extended ROM només té 4 Cabs: Disable Cert, MMS Folder Creation, MIDlet Java Session Fix i Total Commander, i es pot [fer visible](ftp://xda:xda@ftp.xda-developers.com/Wizard/Unlock/Unlock%20Ext%20Rom%20Wizard.cab), i com esta pràcticament buida, pots posar-li el que vulguis. Li he posat el magic button, el flash7, el agilemessenger, el launcher i els operators settings de T-Mobile Nederlands, que és lo que li pot anar be al propietari de la Wizard.

També li he customitzat algunes cosetes amb el [registry wizard](http://forum.xda-developers.com/viewtopic.php?t=39725&start=0), molt útil per als que tingueu aquesta PDA, ja que integra molts hacks per al registre accesibles des-de un menú.

La rom arranca rapidísima, segons el que he llegit al forum soluciona tots els problemes amb el bluetooth i no porta res que "Overloadeje" la pda... llesta per a instalar-li el que vulguis :D

### HTC BlueAngel

Feia temps que no mirava com havia anat evolucionant el port de Linux per a aquesta PDA, la veritat és que en els últims mesos no han avançat molt, tret del kernel, que ara hi ha el 2.6.16 i quan ho vaig estar mirant l'últim era el 2.6.12. El build més actual és el que [va fer rob\_w el 6 d'Agost](http://www.gnulinux.biz/files/blueangel/people/rob_w/), que inclou una imatge de GPE bastant decent i el Gomunicator (l'aplicació de telèfon) que funciona bastant bé, la "putada" és que encara no funciona el wifi, tot i que la BlueAngel porta el mateix xipset que la Universal i aquesta última sí que té el wifi funcionant, el problema és que a tots els dispositius que porten el TIACXWLAN l'accés es fà a través del bus PCI, però en el cas concret de la BlueAngel és a través de PCMCIA, i encara no està clar com accedir-hi. El teclat segueix funcionant igual, li falten tecles ja que no es pot utilitzar la tecla de funció per fer caràcters especials :(

En [aquesta pàgina](http://handhelds.org/moin/moin.cgi/BlueAngel) del wiki de handhelds.org podeu seguir el progrés de la BlueAngel, i [aquí la resta de dispositius d'HTC](http://handhelds.org/moin/moin.cgi/HTC_2dPhones), faltava la Hermes, però vaig crear jo [la pàgina](http://handhelds.org/moin/moin.cgi/HTCHermes) i el link, tot i que com us he dit, de moment hi ha molt poca info :(

Pel que sembla, la [Universal](http://handhelds.org/moin/moin.cgi/Universal) és actualment el dispositiu amb millor suport per linux, el punt que té a fabor és que funciona el wifi, els punts en contra són que no funciona el telèfon ni el power management, per tant no es pot suspendre i es menja la batería molt ràpidament.

Tornem al _lado oscuro_, fa temps que existeixen ROMs no oficials de WM5 per a la BlueAngel que ve de serie amb un WM2003SE, però encara no havia gossat a provar-ne mai cap, però aquests dies que he estat investigant com funcionen les roomtools per fer reverse engeneering de la Hermes he acabat anant a la documentació de altres PDAs per veure si podia aprofitar algo i m'he posat bastant al dia del estat de les ROMS de WM5 per a BlueAngel... i finalment m'he decidit a provar-ne una, evidenment la última, la més _bleeding edge_ que he trobat al moment de fer l'upgrade: es tracta de la [Helmi BA WM2k5 AKU3.2 Beta (v.1.3 Beta)](http://forum.xda-developers.com/viewtopic.php?t=59029), que funciona bastant bé, tot i que tot va una mica més lent que en WM2003SE, però després d'ajustar-la una miqueta he aconseguit que sigui 100% usable i sense retards, va molt bé treure-li el sò dels events i del keypad del telèfon. Si algú més es vol aventurar, és molt important abans de fer l'upgrade a WM5 reparticionar la extended rom i fer-la de 128K, ja que amb les roms de WM5 per a BlueAngel la extended rom es llegeix d'una carpeta a la SD i així podem aprofitar l'espai de la ExtROM com a memòria llirue. Si no reparticionem la ExtROM ens quedarán 43Mb de memòria després de fer l'upgrade, i si ho fem ens quedaràn 60Mb. És molt important fer-ho abans, perqué això no es pot fer desde WM5 i si no ho fem i després volem recuperar l'espai haurèm de downgradejar a WM2003SE per fer-ho. El procés és simple, hem de baixar el <tt>xda-developers_Unlocker.CAB</tt> (buscar-lo als forums de xda-developers) per fer el unhide i unlock de la ExtROM, amb el típic <tt>ExtendedROMUnlocker.arm.CAB</tt> a mi no em funcionava, només em feia el unhide, però no el unlock i per tant al estar en read\_only no la podía reparticionar. Un cop tenim la ExtROM unhided i unlocked només cal executar <tt>repart_doc.exe</tt> (inclòs dins de la rom de Helmi), i dir-li que la volem reparticionar al tamany mínim que ens deixa (128k), apretém el botó de "Format now!" i quan acabe fem un soft reset, donarà un error però diem OK i llestos, ja podém passar a fer el upgrade a WM5. Mirem els detalls de la BlueAngel amb <tt>GetDeviceData.exe</tt>, la meua PDA2k és el model PH20B i el operador que tinc que usar és "CDL" (la vaig comprar lliure) això pot variar en el cas de que sigue brandejada per un operador (t-mobile, vodafone, etc...), amb aquests valors hem d'executar l'script SetOperator.bat correspoenent al model que tinguem, en el meu cas així:

```
C:\Helmi\_WM2k5\_AKU3.2\_32mb\_v1.3\PH20B\>SetOperator.bat CDL Setting Operator to [CDL] for ROM updated checksum to eb1f848d Completed C:\Helmi\_WM2k5\_AKU3.2\_32mb\_v1.3\PH20B\>
```

Creem la extended rom i la copiem a la SD, el procés per crear-la està explicat en un .txt dins de la carpeta, és molt simple, només s'han d'esborrar uns quants fitxers (customitzacions d'operador que no necessitem) i copiar la resta a la carpeta EXTROM de la SD. Un cop fet això, ja podem posar la BlueAngel en mode BootLoader i flashejar de forma normal amb la Rom Upgrade Utility (RUU), si tot va bé fem un hard reset i pulsem Record+Camera ràpidament (les tecles canvien si en lloc de tenir un PH20B tenim un PH20B1, més info [al punt 11 d'aquesta pàgina)](http://wiki.xda-developers.com/index.php?pagename=BA_5.1.1700_build_14343_Upgrade) i veurem una pantalla on hem de marcar que "SI" a tot, seguidament arrancarà WM5 per primer cop i instal·lara la ExtROM que li hem posat a la SD.

Finalment he actualitzat la Radio ROM a la versió 1.15, seguint aquestes instruccions (Second Method Type I), i la PDA ha quedat així:

`
Ram size: 96Mb
Flash size: 32Mb
Operator version: AKU3.2
ROM version: 5.04.06.WWE
ROM date: 03/02/05
Radio version: 1.15.00
Protocol version: 1337.45
ExtROM version: 1.3
Windows version: WM5.0 with the Messaging and Security Feature Pack OS 5.1.195 (Build 15605.3.2.0)
`

### That's all folks!

Bueno, si has aguantat el rollo fins aquí sense tirar avall el scroll felicitats... has d'estar molt aburrit ;) jeje :P Ara en serio, m'ha quedat un post molt llarg, massa tècnic, i una mica desestructurat, però no tinc temps i necessito dormir una mica, així que per a qualsevol dubte estàn els comentaris :D

