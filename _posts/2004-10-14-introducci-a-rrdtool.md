---
layout: post
title: Introducció a rrdtool
date: 2004-10-14 23:30:00.000000000 +02:00
type: post
categories:
- linux
- pofHQ
tags: []
meta:
  tags: ''
permalink: "/2004/10/14/introducci-a-rrdtool/"
---
Fins ara havía utilitzat mrtg per a monitoritzar el server i el firewall de la xarxa de casa, però fa uns temps vaig decidir canviar el sistema de monitorització i reimplementar-ho de nou amb rrdtool. Al principi em va semblar una mica complicat, sobre tot per la sintàxis que té que és una mica farragosa, però al final he descobert que per fer les 4 gràfiques típiques i alguna coseta més no porta massa complicació, això si, hem de dedicar una mica de temps a llegir-nos la documentació basica i fer bastantes proves per a trovar la configuració "ideal". Com això és una d'aquestes coses que s'oblida ràpit a mida que passa el temps, sobre tot si no ho estàs tocant cada dia, faig un resum dels punts més bàsics ara que ho tinc _fresquet_ i el guardo com a referència per si algún dia haig de monitoritzar alguna altra cosa, i així de pas li pot servir d'exemple a qui es vulgui iniciar en el tema.

[Round Robin Database Tool](http://people.ee.ethz.ch/~oetiker/webtools/rrdtool/) (rrdtool) és un programa que permet almacenar dades numériques al llarg del temps i representarles en forma de gràfiques, de manera similar al [mrtg](http://people.ee.ethz.ch/~oetiker/webtools/mrtg/) però molt més ràpid i flexible.

<!--more-->

Abans de res hem de decidir quines son les dades que volem monitoritzar, en aquest exemple jo he decidit fer una gràfica amb el SAI, monitoritzant el temps que aguantaría la batería si marxés la llum i el tant percent de càrrega que té la bateria. Poso dos valors per fer-ho més simple, però amb rrdtool podem mostrar en una gràfica tants valors com vulguem.

En resum haurém de fer 3 coses:

- Crear la _Round Robin Database_ (RRD) on almacenarem les dades
- Recollir les dades i introduir-les a la RRD periòdicament
- Generar les gráfiques amb les dades recopilades, també periòdicament

Per a cadascuna d'aquestes tres coses utilitzarem un script, jo els he fet amb _bash_, però es pot utilitzar qualsevol cosa (perl, python, php, etc...).

**1)** Per a crear la RRD executarem la comanda `rrdtool create` amb les següents opcions:

```
rrdtool create ups\_timeload.rrd --start `date +"%s"` --step 300 DS:timeleft:GAUGE:600:0:100 DS:loadpct:GAUGE:600:0:100 RRA:AVERAGE:0.5:1:600 RRA:AVERAGE:0.5:6:700 RRA:AVERAGE:0.5:24:775 RRA:AVERAGE:0.5:288:797 RRA:MAX:0.5:1:600 RRA:MAX:0.5:6:700 RRA:MAX:0.5:24:775 RRA:MAX:0.5:288:797
```

Amb aquesta comanda, indiquem a rrdtool que volem crear una nova RRD, amb el nom `ups_timeload.rrd`, que començarem a posar dades a partir del moment de creació de la RRD (no acceptará valors anteriors a aquest timestamp), i que anirem introduint noves dades a la RRD cada 300 segons (5 minuts). Després definim els _Data Sources_ (DS), que en aquest cas son _timeleft_ i _loadpct_ (cada RRD pot almacenar dades de múltiples DS).

Cada DS el definim de la següent manera:

```
DS:ds-name:DST:heartbeat:min:max
```

On `ds-name` correspón al nom del DS, DST és el _Data Source Type_, es a dir, el tipus de dada que almacenarem, els tipus poden ser GAUGE, COUNTER, DERIVE i ABSOLUTE. Per a més informació sobre els tipus de dades mirar el man de `rrdcreate(1)`. El heartbeat es un interval temps en segons, en el nostre exemple si la RRD no rep un nou valor per a un DS durant aquest temps almacenará el valor com a UNKNOWN (desconegut), i finalment min i max indiquen els valors mínim i màxim que podràn tenir les dades que almacenem, en el exemple el valor minim l'he posat a 0 i el máxim a 100, ja que el % de carrega mai superarà el 100% i el temps de duració de la bateria --almacenat en minuts-- mai passará de 100 minuts. Si una dada sobrepassa aquests limits será enmagatzemada amb valor UNKNOWN.

Les següents linies declaren el _Round Robin Archive_ (RRA), per a una definició exacta aconsello mirar el man de `rrdcreate(1)`. Per a la majoría de casos si volem extreure gràfiques de dia, setmana, mes i any només haurém de copiar els valors que he posat en l'exemple, per a que s'entengui ho explico per sobre agafant aquesta línea com a exemple:

```
RRA:AVERAGE:0.5:288:797
```

Definim un RRA que calculará la mitja (AVERAGE) dels últims 288 valors almacenats, com cada valor s'agafa cada 300 segons tindrem el valor mig d'un día (288 vegades 300 segons, son 24 hores). Si cada día agafem aquest valor podrem representar un any complert amb les mitges diaries. A part de AVERAGE, podem utilitzar també les funcions de consolidació MAX, MIN i LAST. El 0.5 indica que fer quan trovem un valor UNKNOWN, per mes info mirar el man ;) i el 797 indica quants d'aquests valors calculats s'almacenaran en un RRA.

**2)** Un cop la RRD està creada, hem d'anar introduint-li les dades cada 300 segons. Això ho podem fer de moltes maneres diferents, per exemple un script en un cron o un script que ell mateix contingui un bucle amb un sleep... la finalitat és recollir les dades que volem enmagatzemar (per exemple dades extretes amb `snmpget`, o la sortida numérica de qualsevol altra comanda) i passar-li-les a rrdtool amb la comanda `rrdtool update`. A continuació el meu exemple:

```
#!/bin/bash function extract { /usr/sbin/apcaccess status |grep ^$1 |awk '{print $3}' } DS1=`extract TIMELEFT` DS2=`extract LOADPCT` if [[$DS1 == '']] ; then DS1=U ; fi if [[$DS2 == '']] ; then DS2=U ; fi rrdtool update ups\_timeload.rrd N:$DS1:$DS2
```

El que fa aquest script és capturar els valors de TIMELEFT i LOADPCT que extreu de la comanda `apcaccess` que controla el SAI, un cop te aquests valors els passa com a paràmetres a `rrdtool update` per introduir-los a la RRD que hem creat abans.

**3)** Quan ja alimentem la RRD amb dades només ens queda representar-les de forma visual per poder-les veure, així necessitarem alguna manera de generar les gràfiques amb la informació recopilada, per a fer-ho utilitzarem un script -- en qualsevol llenguatge, igual que en el cas anterior -- aquí teniu el meu exemple:

```
rrdtool graph ups\_timeload\_day.png --start=-86400 --end=-300 --title="UPS Time Left and Load Percent - day (5 min avg)" --base=1000 --height=120 --width=500 --alt-autoscale-max --lower-limit=0 --vertical-label="time (min) / load (%)" DEF:timeleft="ups\_timeload.rrd":timeleft:AVERAGE DEF:loadpct="ups\_timeload.rrd":loadpct:AVERAGE AREA:loadpct#0000FF:"UPS Load " GPRINT:loadpct:LAST:" Current:%8.2lf %s(%%)" GPRINT:loadpct:AVERAGE:" Average:%8.2lf %s(%%)" GPRINT:loadpct:MAX:" Maximum:%8.2lf %s(%%)n" LINE3:timeleft#FF0000:"Time Left" GPRINT:timeleft:LAST:" Current:%8.2lf %smin" GPRINT:timeleft:AVERAGE:" Average:%8.2lf %smin" GPRINT:timeleft:MAX:" Maximum:%8.2lf %sminn"
```

El que fem es invocar a rrdtool, dient-li que ens fassi una gràfica que comenci fa 86400 segons (un dia) i que acabi 300 segons abans de la hora actual (5 minuts), i li indiquem altres paràmetres com el titol, les dimensions o la llegenda de l'eix vertical. La part que sembla més complicada, realment no ho és tant, a continuació us explico que volen dir totes aquestes coses:

Primer definim les variables _timeleft_ i loadpct, extraient les dades de la RRD on les hem almacenat anteriorment, això ho fem en les dues linees que començen per DEF. A continuació diem que volem dibuixar una AREA de color #0000FF (blau) amb el valor de _loadpct_, a la llegenda això apareixerà amb el titol "UPS Load". A continuació imprimim amb GPRINT els valors LAST, AVERAGE i MAX del tant percent de carrega del SAI, després imprimim el _timeleft_ en una línea de grosor 3 i els valors LAST,AVERAGE i MAX.

En aquest exemple hem vist com representar els valors en una àrea i en una línea. Els valors de les diferents variables es poden representar de 5 maneres diferents, AREA, LINE1, LINE2, LINE3 i STACK. L'AREA es una area solida colorejada, STACK és molt similar a l'àrea, amb la diferencia de que no es sobreposa amb una àrea previament representada sino que es col·loca al damunt. LINE1/2/3 és una linea més o menys groixuda en funció del número que posem.

Molts cops no ens interessa dibuixar dirèctament el valor que obtenim, sino que volem fer alguns calculs sobre aquest valor (passar de Mbits a MBytes, de graus celsius a centigrads, sumar 3 valors per dibuixar un total, etc...), per a això utilitzarem CDEF's, la seua sintàxis és una mica complexa al principi, però no hi ha rés millor que uns quants exemples i un bon manual, així que us aconsello que us mireu el [CDEF Tutorial](http://people.ee.ethz.ch/~oetiker/webtools/rrdtool/tutorial/cdeftutorial.html) si voleu veure uns quants exemples.

I bàsicament això és tot, ara només em queda explicar la solució a un problema "tipic" que a mi m'ha donat una mica de mal de cap, així si a algú li passa el mateix ja sabrà com solucionar-ho:

PROBLEMA: Tenim un fitxer rrd amb 1 (o més) DS COUNTER, però el contador es reseteja i obtenim un pic molt alt a la gràfica. Hi ha 2 solucions:

- A l'script que utilitzem per dibuixar la gràfica, tenim el següent:

`DEF:A=file.rrd:dsname:AVERAGE`

El que hem de fer és dir-li que no volem que ens representi valors entre un mínim i un màxim, això ho fem de la següent manera:

`CDEF:B=A,MINVALUE,GT,A,MAXVALUE,LT,A,UNKN,IF,UNKN,IF`

Llavors, en lloc de dibuixar A, dibuixem B. Cal tenir en compte que a la línea anterior s'han de reemplaçar MINVALUE i MAXVALUE pels valors mínims i màxims que es vulguin dibuixar.

La segona sol·lució consisteix en reparar el fitxer RRD que conte els valors de pic que ens fan malbé la gràfica (a mi personalment m'agrada més l'altra sol·lució, pero aquí va aquesta, per si algú li pot ser útil):

- Primer hem de fer un backup del fitxer RRD, llavors executem el següent:

`rrdtool tune file.rrd
rrdtool tune file.rrd --data-source-type dsname:DERIVE`

Amb això canviem el tipus de dada de COUNTER a DERIVE, despres continuem:

`rrdtool tune file.rrd --minimum dsname:MINVALUE --maximum dsname:MAXVALUE`

Aquí també cal canviar MINVALUE i MAXVALUE pels valors mínim i màxim que vulguem dibuixar, seguim:

`
rrdtool tune file.rrd
rrdtool dump file.rrd > file.xml
rrdtool restore -r file.xml file.rrd.new
`

I això és tot, aquí us deixo [els scripts que jo utilitzo](/archives/files/rrdtool-scripts.tgz) per a generar [aquestes gràfiques](/graphs/).

