---
layout: post
title: Solució anti-spam
date: 2005-06-26 03:36:20.000000000 +02:00
type: post
categories:
- linux
- pofHQ
tags: []
meta:
  tags: spam antispam spamassassin meme
permalink: "/2005/06/26/solucio-anti-spam/"
---
Seguint amb la moda dels _memes_ bloguerils, en [r0sk](http://www.userlinux.net) m'ha passat el testimoni del [meme anti-spam](http://www.userlinux.net/noticia.527.php) iniciat per en [Juanjo](http://blackshell.usebox.net/archivo/585.php) a [blackshell](http://blackshell.usebox.net/).

### Mesures contra spam

La solució que tinc montada per eliminar el correu brossa i els virus es basa en [sendmail](http://www.sendmail.org/) + [SpamAssassin](http://www.spamassassin.org/) + [procmail](http://www.procmail.org/) + [f-prot](http://www.f-prot.com/).

Al `/etc/procmailrc` faig passar tots els missatges entrants de tamany inferior a 650k per l'spamassassin (pipe a `spamc`) i tots els de tamany inferior a 2,5Mb per l'antivirus. La restricció de tamany es per evitar carregar massa el servidor.

A més, quan detecto un virus modifco la capçalera i el subject del missatge amb `formail` (inclòs dintre del paquet procmail) per avisar al receptor.

Si us fixeu en el procmailrc, veureu que no invoca directament a l'antivirus, sino a un script que s'encarrega de decidir si el missatge és un virus o no, aquest script és el `/etc/local/isvirus`, que bàsicamnet escriu el contingut del missatge en una carpeta temporal, passant-lo per `uudeview` per decodificar-lo i extreure els diferents attachments en cas de que sigui un missatge MIME en _multi-part_. Finalment, en funció del codi de retorn del f-prot li passa un 0 (false) o un 1 (true) a procmail per a que entri a la condició de reescriure la capçalera si s'ha detectat un virus o no.

Aquesta és la configuració _system-wide_, és a dir, per a tothom que utilitza el meu servidor de correu. Ara us explico que faig jo amb els correus un cop han passat per el spamassassin i el f-prot.

Al fitxer `~/.procmailrc` tinc el següent:

```
# Enviem els virus a la carpeta de virus :0 H \* ^X-Warning: VIRUS DETECTED by pof.eslack.org .Virus/ # Enviem el spam a la carpeta de spam :0 H \* ^X-Spam-Flag: YES .Spam/
```

Això el que fa és mirar la capçalera del missatge, i si conté el warning que hem afegit abans per als virus, envia el missatge a la carpeta "Virus", i si conté la marca del spamassassin (X-Spam-Flag: YES) llavors l'envia a la carpeta "Spam".

NOTA: Com marquem les capçaleres dels correus, també podem afegir les marques X-Warning i X-Spam-Flag a la configuració del [mailman](http://www.gnu.org/software/mailman/) en cas de que gestionem llistes de correu.

Per _gestionar_ els falsos positius i els falsos negatius, a l'estructura de carpetes IMAP tinc dos directoris dintre de la carpeta Spam: `learn-spam` i `learn-ham`. Quan un mail que és spam es cola a la INBOX simplement el moc a la carpeta `learn-spam`, i quan passa al revés (que no passa casi mai), es a dir, quan un mail que no és spam és detectat com a spam per l'spamassassin i se'n va a la carpeta Spam, el moc a la carpeta `learn-ham`.

Aquestes dues carpetes són processades per l'script `/etc/local/processa-spam.sh` que s'executa cada hora des d'un cron. Aquest script el que fa és indicar a spamassassin que _s'ha equivocat_ (entrenar el filtre bayessià) i moure el missatge a la carpeta correcta, passant-lo de nou pel procmail en cas de que sigui _ham_, per assegurar-nos de que el filtre ha quedat ben entrenat, ja que de vegades és necessari moure el missatge un parell o tres de cops abans de que spamassassin aprengui.

### Quantitat de spam rebut ahir

Primer el spam detectat per l'spamassassin:

`
$ cat mail.log.0 |grep "^Jun 25" |grep "identified spam" |wc -l
212
`

A part d'aquest spam que arriba a mailboxes vàlides, el servidor de correu para la resta de mail que arriba a mailboxes no existents dins del domini <tt>eslack.org</tt>:

`
$ cat mail.log.0 |grep "^Jun 25" |grep "@eslack.org... User unknown" |wc -l
2520
`

Això són logs d'aquest estil:

```
Jun 25 23:56:38 s0 sm-mta[11567]: j5PLucR4011567: jmwshixyowwmd @eslack.org... User unknown Jun 25 23:57:12 s0 sm-mta[11739]: j5PLvCC8011739: luinjvhpxvug @eslack.org... User unknown Jun 25 23:58:31 s0 sm-mta[12167]: j5PLwUqa012167: pjvrjulzq @eslack.org... User unknown Jun 25 23:58:38 s0 sm-mta[12192]: j5PLwcOY012192: bzkacfl @eslack.org... User unknown
```

### Temps que li dedico al spam

Donçs durant la meva vida li'n he dedicat més del que m'agradaria, però aquesta solució que ja fa un temps que la tinc funcionant n'estic bastant content. Diariament no li dedico temps al spam, només quan veig que la carpeta spam te més de 1000 missatges no llegits, llavors faig un scroll ràpid per veure que no s'hagi colat res important. Calculo que més o menys dec dedicar-li uns 5 minuts un cop a la setmana...

### Passant el testimoni

Donçs aquest cop li passo el testimoni a:

1. [Oriol - Oriol News Portal](http://oriol.joor.net)
2. [Esteve - TizOS](http://esteve.tizos.net)
3. [Sergio - Tripledes Journal](http://www.livejournal.com/users/tripledes/)
4. [Marc - Noctambuls](http://www.noctambuls.net/)
5. [David - David Martos](http://www.davidmartos.com/)
6. [Carlos - Topopardo](http://weblog.topopardo.com/)

Com sempre, podeu fer _trackbacks_ si seguiu amb el _meme_...

