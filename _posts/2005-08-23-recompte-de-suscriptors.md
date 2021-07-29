---
layout: post
title: Recompte de suscriptors
date: 2005-08-23 00:03:20.000000000 +02:00
type: post
categories:
- pofHQ
tags: []
meta:
  tags: feed blog subscriber count
permalink: "/2005/08/23/recompte-de-suscriptors/"
---
Després de llegir el post [Contando suscriptores](http://blackshell.usebox.net/archivo/628.php) de Juanjo he decidit fer una aproximació de la gent que està suscrita als feeds d'aquest blog. El primer problema que se'm ha plantejat és saber quantes URLs de feeds diferents tinc, ja que Wordpress (el sistema de publicació que utilitza el blog) permet generar diferents feeds en diferents formats i en URLs diferents segons si accedim a la URL generada amb <tt>mod_rewrite</tt> o no.

He agafat com a base els logs d'apache del mes de Juliol, i he executat la següent comanda per obtenir la llista de feeds disponibles:

```
# cat july.log |awk '{ print $7 }' |egrep "^/blog/.\*(rss|atom|rdf|feed)" |sort -u \>llista-feeds
```

He editat l'arxiu llista-feeds manualment, per extreure el que realment no eren feeds, ja que degut a la expressió regular que he utilitzat s'han colat artícles com [aquest](/blog/2004/10/24/feed-overflow/).

Després he fet un llistat només dels logs de peticions de feeds del mes de juliol, eliminant les peticions dels buscadors principals d'aquesta manera:

```
# for f in `cat llista-feeds` ; do cat july.log |grep -w "GET $f" |egrep -vi "(crawl|bot|slurp)" ; done \> feed-log
```

A continuació he volgut extreure varies dades d'aquest log:

- El nombre de suscriptors per feed
- El nombre total de suscriptors
- La cadena <tt>user agent</tt> que identifica l'agregador de feeds utilitzat

<!--more-->

### El nombre de suscriptors per feed

Extrec el llistat de les IPs úniques que consulten els feeds:

```
# cat feed-log |awk '{print $1}' |sort -u \> ips-uniques
```

Creo un fitxer de logs per a cada ip única:

```
# mkdir tmp # for f in `cat ips-uniques` ; do cat feed-log |grep ^$f \>\> tmp/$f ; done
```

Suposo que un suscriptor no és un suscriptor si no ha demanat feeds al menys 10 cops durant un mes desde una IP única:

```
# cd tmp tmp # wc -l \* |grep -v total |awk '{printf("%d %s\n",$1,$2) }' |egrep -v "^([1-9]) "
```

Moc tots els logs que **de moment** _considero_ suscriptors a un altre directori:

```
tmp # mkdir ../tmp2 tmp # for f in `wc -l * |grep -v total |awk '{printf("%d %s\n",$1,$2) }' |egrep -v "^([1-9]) " |cut -f 2 -d " " ` ; do mv $f ../tmp2/ ; done
```

De tots aquests, només considero suscriptors els que han agafat el feed al menys durant 4 dies diferents, així que repeteixo la mateixa operació movent a una altra carpeta tots els logs que considero vàlids:

```
tmp # cd ../tmp2 tmp2 # for f in `ls` ; do days=`cat $f |awk '{print $4}' |cut -f 1 -d "/" |sort -u |wc -l` ; echo "$days $f" ; done |egrep -v "^([1-3]) " |cut -f 2 -d " " \> ../suscriptors-ok tmp2 # mkdir ../tmp3 tmp3 # for f in `cat ../suscriptors-ok` ; do mv $f ../tmp3 ; done
```

Amb això ja tinc el llistat de les IPs úniques que han demanat un feed al menys 10 cops durant un mes durant com a mínim 4 dies. Fent un `ls |wc -l` al directori `tmp3` surten un total de 162 IPs úniques, però aquest no és el nombre _real_ de suscriptors, ja que hem de tenir en compte els agregadors web com [bloglines](http://www.bloglines.com) o [rojo](http://www.rojo.com).

En el cas de bloglines és ben senizll, ja que ens informa del nombre de suscriptors en el <tt>user agent</tt>, per veure quants suscriptors tinc per feed a bloglines he utilitzat el següent mètode:

```
tmp3 # grep -i bloglines \* |awk '{printf("%s %s %s\n",$7, $14,$15)}' |sed -e 's/)"$//' |sort -u /blog/category/podcast/rss2/ 19 subscribers /blog/category/podcast/rss2/ 20 subscribers /blog/category/podcast/rss2/ 21 subscribers /blog/comments/feed/rss2/ 6 subscribers /blog/comments/feed/rss2/ 7 subscribers /blog/feed/atom/ 27 subscribers /blog/feed/atom/ 28 subscribers /blog/feed/rss2/ 33 subscribers /blog/feed/rss2/ 34 subscribers /blog/feed/rss2/ 35 subscribers
```

Amb això veiem que hi ha 4 URLs de feeds a les que tinc suscriptors de bloglines, i per a cada URL el nombre de suscriptors que té. El fet de que apareixin repetits és perque el nombre de suscriptors ha variat durant el mes, així per exemple a principis de juliol hi havia 33 persones suscrites al feed rss2 i a finals 35.

Anem a veure ara el nombre de suscriptors per feed:

```
tmp3 # for f in `cat ../llista-feeds` ; do ips=`grep "$f " * |cut -f 1 -d ":" |sort -u` ; total=`echo $ips |sed -e "s/ /\n/g" |wc -l` ; if [$total != 1] ; then echo "$f $total" ; fi ; done /blog/2004/01/06/40/feed 2 /blog/category/podcast/rss2 88 /blog/comments/feed/rss2 9 /blog/feed 12 /blog/feed/atom 36 /blog/feed/rdf 4 /blog/feed/rss 11 /blog/feed/rss2 86
```

Si ajuntem aquestes dades amb les que hem obtingut anteriorment de bloglines, obtenim la informació que voliem inicialment:

| **Feed** | **Suscriptors** |
| /blog/2004/01/06/40/feed | 2 |
| /blog/category/podcast/rss2 | 108 |
| /blog/comments/feed/rss2 | 15 |
| /blog/feed | 12 |
| /blog/feed/atom | 63 |
| /blog/feed/rdf | 4 |
| /blog/feed/rss | 11 |
| /blog/feed/rss2 | 120 |

### El nombre total de suscriptors

És molt comú que una persona estigui sindicada per exemple al feed de comentaris i al rss general del blog, per tant, per obtenir una aproximació del nombre total de suscriptors només tindré en compte els feeds principals (rss2, rss, rdf i atom) i exclouré els feeds de posts sueltos, comentaris i podcast. Si sumem els suscriptors d'aquests feeds dona un total de 210, però cal tenir en compte que una única IP pot estar sindicada a més d'un feed, aixi que ho farem de la següent manera:

Miro quantes IPs úniques estan suscrites a feeds principals:

```
tmp3 # for f in `ls ` ; do feeds=`cat $f |awk '{print $7}' |grep -v podcast |grep -v comments |grep -v 2004 |grep -v 2005 |sort -u` ; if [-n "$feeds"]; then echo "$f" ; fi ; done |wc -l 112
```

A aquest nombre cal sumar els 28 (atom) + 35 (rss2) que havíem extret de bloglines, i restar-li 3 (la IP pública amb la que bloglines fa la petició del feed, i jo mateix q estic suscrit als 2 feeds de bloglines), per tant suma un total de **171** suscriptors únics.

### Agregador de feeds utilitzat

Per extreure el llistat de tots els agregadors utilitzem la següent comanda sobre els fitxers de log:

```
tmp3 # cat \* |cut -f 6 -d '"' |sort -u
```

Per veure quins <tt>user agents</tt> són els més utilitzats podem executar aquest script:

```
#!/bin/sh cat tmp3/\* |cut -f 6 -d '"' |sort -u \> ./tmpfile numlines=`cat ./tmpfile |wc -l` for (( i=1;$i\< =$numlines;i++ )); do USER\_AGENT=`head -n $i ./tmpfile |tail -n 1` TOTAL=`grep " \"$USER_AGENT\"$" tmp3/* |cut -f 1 -d ":" |sort -u |wc -l` echo "$TOTAL $USER\_AGENT" done
```

En el meu cas, agrupant a _groso modo_ les diferents versions surten auqests valors:

| **Agregador** | **Usuaris** |
| Firefox | 69 |
| Mozilla | 44 |
| Liferea | 27 |
| FeedDemon | 10 |
| NewsFire | 8 |
| AppleSyndication | 6 |
| iTunes | 6 |
| Feedreader | 4 |
| NetNewsWire | 3 |

I això és tot, si has llegit fins aquí linea per linea felicitats per aguantar el _rollo_ de post que m'ha sortit!

