---
layout: post
title: Sistema de backup
date: 2005-09-20 20:29:16.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: backup unix administration
permalink: "/2005/09/20/sistema-de-backup/"
---
Després de [tots](/blog/2005/08/02/em-falla-un-disc-del-raid1/) [els](/blog/2005/09/04/el-disc-ha-petat/) [percances](/blog/2005/09/20/recuperant-una-particio-ext3/), he implementat un sistema de backup amb la ferramenta [Flexbackup](http://www.flexbackup.org/).

Com el seu nom indica, és una utilitat molt _flexible_ que permet combinar diferents programes al nostre gust per fer els backups: `afio`, `dump`, `tar`, `cpio`, `star`, `pax`, `zip`, `lha`, `ar`, `shar`... a més permet comprimir els backups i enviar-los per xarxa utilitzant protocols segurs.

El fitxer de configuració de <tt>flexbackup</tt> es troba situat a `/etc/flexbackup.conf`, aquests son els canvis que jo he fet respecte al fitxer de configuració original amb els paràmetres per defecte:

<!--more-->

```
#especifiquem el tipus d'arxiu $type = 'tar' # set de directoris dels que farem backups $set{'base'} = "/bin /boot /dev /etc /lib /opt /sbin"; $set{'usr'} = "/usr"; $set{'var'} = "/var"; $set{'tmp'} = "/tmp"; $set{'homes'} = "/home/pof /root"; $set{'homepau'} = "/home/pau /home/public/pau"; # no utilitzo cinta, faig el backup amb fitxers estàtics sobre un directori $buffer = 'false'; $device = '/backup/data'; $staticfiles = 'true'; # no vull fer backup del distfiles $exclude\_expr[0] = './portage/distfiles/.\*'; # canvio les rutes per defecte $logdir = '/backup/log/flexbackup'; $tmpdir = '/backup/tmp'; $stampdir = '/backup/timestamp/flexbackup'; $index = '/backup/timestamp/flexbackup/index';
```

NOTA: si algú vol el fitxer de configuració complert ~~només l'ha de demanar~~ el pot descarregar [aquí](/archives/files/flexbackup.conf).

Un cop tenim el flexbackup configurat, només ens queda fer que s'executi periòdicament per a que es realitzen els backups del sistema. Jo m'he fet un _shell script_ per a fer un backup **complert** un cop al mes, un backup **diferencial** (només fitxers que han canviat o s'han afegit des de l'ultim backup complert) un cop a la setmana i un backup **incremental** (només fitxers que han canviat o s'han afegit des de l'últim backup de qualsevol tipus) un cop al dia. El codi del script el teniu a continuació:

```
#!/bin/sh SET="all" EMAIL="luser@example.com" WEEKDAY=`date +%w` MONTHDAY=`date +%e` # mount backup partition mount /dev/hdc1 /backup/ if ["$?" != "0"]; then mail -s "ERROR MOUNTING /dev/hdc1" $EMAIL \< /dev/null exit -1 fi # Daily incremental backups weekdays (catch day-to-day changes) # Weekly differential backups Wednesday (catch all changes since full) # Full backup - once a month on Thursday only if ["$WEEKDAY" == "4"] && [$MONTHDAY -le 7] ; then /usr/bin/flexbackup -set ${SET} -level full elif ["$WEEKDAY" == "3"]; then /usr/bin/flexbackup -set ${SET} -level differential else /usr/bin/flexbackup -set ${SET} -level incremental fi # umount backup partition cd / umount /backup/
```

Finalment hem de posar el _shell script_ en un `cron` per a que s'execute diàriament, jo l'he programat cada dia a les 4:31 de la matinada, amb aquesta línia al <tt>crontab</tt> del root:

```
# flexbackup tots els dies a les 4:31 31 4 \* \* \* /root/bin/backup.sh \>/dev/null 2\>&1
```

I això és tot, s'accepten suggeriments i comentaris ;)

