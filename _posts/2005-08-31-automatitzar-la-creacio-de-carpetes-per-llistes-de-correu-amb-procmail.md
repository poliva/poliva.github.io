---
layout: post
title: Automatitzar la creació de carpetes per llistes de correu amb procmail
date: 2005-08-31 16:03:32.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: procmail filter recipe email
permalink: "/2005/08/31/automatitzar-la-creacio-de-carpetes-per-llistes-de-correu-amb-procmail/"
---
En aquest post trobareu una recepta molt útil per als que utilitzeu [procmail](http://www.procmail.org/) per organitzar / filtrar el correu, el que fa és crear automàticament una carpeta dins del _maildir_ del usuari cada cop que ens donem d'alta a una nova llista de correu, i posteriorment classificar tots els missatges procedents de la llista en aquesta nova carpeta. La carpeta rep automàticament el nom de la llista de correu.

Funciona amb quasi tots els gestors de llistes de correu que trobareu por ahi: mailman, majordomo, ezmlm,... i si amb algo no funciona és molt fàcil d'adaptar el filtre mirant les capçaleres del correu procedent de la llista. La gràcia que té és que no cal editar el `.procmailrc` cada cop que ens donem d'alta a una nova llista per classificar-la en una carpeta separada, amb aquest filtre tot s'organitza automàtic.

<!--more-->

```
:0 \* ^X-Mailing-List-Name: \/[^@]+ .list.`echo $MATCH | sed -e 's/[\/]/_/g'`/ :0 \* ^Sender: owner-\/[^@]+ .list.`echo $MATCH | sed -e 's/[\/]/_/g'`/ :0 \* ^X-BeenThere: \/[^@]+ .list.`echo $MATCH | sed -e 's/[\/]/_/g'`/ :0 \* ^Delivered-To: mailing list \/[^@]+ .list.`echo $MATCH | sed -e 's/[\/]/_/g'`/ :0 \* ^X-Mailing-List: \< \/[^@]+ .list.`echo $MATCH | sed -e 's/[\/]/_/g'`/ :0 \* ^X-Loop: \/[^@]+ .list.`echo $MATCH | sed -e 's/[\/]/_/g'`/ :0 \* ^X-List-ID: \<\/[^@\.]+ .list.`echo $MATCH | sed -e 's/[\/]/_/g'`/ :0 \* ^X-list: \/[^@\.]+ .list.`echo $MATCH | sed -e 's/[\/]/_/g'`/ :0 \* ^Delivered-To: lista \/[^@]+ .list.`echo $MATCH | sed -e 's/[\/]/_/g'`/
```

El filtre té un "petit" problema si utilitzeu <acronym title="Internet Message Access Protocol">IMAP</acronym>: Si us donen d'alta a una llista de correu sense avisar, no veureu els missatges ja que es col·locaran a una carpeta nova que no tindreu suscripció a través d'<acronym title="Internet Message Access Protocol">IMAP</acronym>. Per sol·lucionar-ho jo utilitzo aquest scriptillo que llanço des d'un _cron_ un cop al dia, per a que m'enviï un correu cada cop que rebo una llista nova:

```
#!/bin/sh TMP="/home/pau/tmp" ls -a --color="no" ~/.maildir/ |grep "\.list\." |sed -e "s/\.list\.//g" |sort \> ${TMP}/mailinglists2.txt diff ${TMP}/mailinglists2.txt ${TMP}/mailinglists.txt \> ${TMP}/mailinglists3.diff if [$? != 0]; then mail -s "new mailing list!" myemail@example.org \< ${TMP}/mailinglists3.diff mv ${TMP}/mailinglists2.txt ${TMP}/mailinglists.txt fi
```

Espero que li sigui útil a algú... :)

