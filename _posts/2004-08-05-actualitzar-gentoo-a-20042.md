---
layout: post
title: Actualitzar gentoo a 2004.2
date: 2004-08-05 03:28:39.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: ''
permalink: "/2004/08/05/actualitzar-gentoo-a-20042/"
---
Una de les coses que més m'agrada de Gentoo és que no cal "reinstalar" cada cop que surt una versió nova de la distribució, cada nova _release_ només es diferència de les versions anteriors en que treuen nous _livecds_, nous _stages_ i nous _snapshots_ de portage. Això vol dir que si anem actualitzant periòdicament el nostre sistema no ens caldrà fer cap canvi per estar al dia. Bé, aixo de "cap canvi" és subjectiu, ja que si volem tenir el <acronym title="conté els virtuals i paquets que s'inclòuen per defecte al system">profile</acronym> de la última distribució haurem de canviar l'enllaç de `/etc/make.profile` per a que apunti al profile adequat; per fer això només caldrà escriure la següent comanda:

```
ln -sf /usr/portage/profiles/default-linux/x86/2004.2 /etc/make.profile
```

i seguidament actualitzar el world amb la opció `-D`, que actualitzarà també totes les dependències.

Si heu instal·lat Gentoo prèviament veureu que amb el nou profile només hi ha un canvi relativament substancial, és el canvi de xfree86 per xorg, sobre tot per temes de llicència.

Més informació [en aquest thread](http://thread.gmane.org/gmane.linux.gentoo.user/91301) de gentoo-user.

