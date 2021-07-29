---
layout: post
title: Configurar sendmail com a Backup MX
date: 2005-08-12 02:42:08.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: sendmail backup relay mail
permalink: "/2005/08/12/configurar-sendmail-com-a-backup-mx/"
---
En Roman m'ha demanat si li podia fer de Backup <acronym title="Mail eXchanger">MX</acronym> durant uns dies, mentre canvía el parquet de casa que ha d'apagar el servidor... El procés de canviar el parquet és molt complicat i no us el puc explicar, pero fer de MX secundari amb sendmail és ben senzill:

Al fitxer `/etc/mail/relay-domains` afegim el domini d'en Roman, per a que Sendmail permeti la entrada dels correus:

```
nopcode.org
```

Si el fitxer no existeix, el creem. Després executem la següent comanda per actualitzar (o crear) la bbdd de relays:

```
# makemap hash /etc/mail/relay-domains \< /etc/mail/relay-domains
```

Al fitxer `/etc/mail/mailertable` afegirem una línia indicant quin és el MX primari al que hem de reenviar el correu que ens arribe per al domini que hem afegit anteriorment com a relay:

```
nopcode.org esmtp:mail.nopcode.org
```

Seguidament, també actualitzem la bbdd mailertable:

```
# makemap hash /etc/mail/mailertable \< /etc/mail/mailertable
```

I això és tot... quan el MX primari (mail.nopcode.org) estigui caigut, el meu servidor recullirà el correu i l'encuarà fins que el primari torni a respondre.

Ah! També cal tenir en compte que per a afegir un MX secundari s'ha de modificar la entrada MX del DNS per al domini en questió. En el cas de nopcode.org ha quedat així:

```
# dig nopcode.org MX ;; QUESTION SECTION: ;nopcode.org. IN MX ;; ANSWER SECTION: nopcode.org. 3584 IN MX 20 pof.eslack.org. nopcode.org. 3584 IN MX 10 mail.nopcode.org.
```
