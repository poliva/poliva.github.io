---
layout: post
title: Corregir l'ebuild d'asterisk per detectar el fi de trucada
date: 2005-10-04 20:42:00.000000000 +02:00
type: post
categories:
- linux
- voip
tags: []
meta:
  tags: asterisk telephony spain polarity
permalink: "/2005/10/04/corregir-lebuild-dasterisk-per-detectar-el-fi-de-trucada/"
---
<p><img src="{{ site.baseurl }}/assets/images/2005/10/asterisk.png" alt="Asterisk" class="esquerra border" />
<p>Fa un tems rroca em va comentar com fer per<br />
<a href="/blog/2005/05/08/configuracio-de-la-tarjeta-x100p-amb-asterisk/#comment-2237">detectar correctament el fi de trucada</a> amb <a href="http://www.asterisk.org">Asterisk</a> sobre una linia analògica.</p>
<p>Avui, Julian J.M. ---l'autor del <em>patch</em>---
ha enviat [aquest missatge](http://groups.google.com/group/asterisk-es/browse_thread/thread/aa185189c44c1cf5/3d1912f09272d0f6) a la llista d'[asterisk-es](http://groups.google.com/group/asterisk-es) ja que ha reobert el [bug](http://bugs.digium.com/view.php?id=3874) amb la petició a Digium per a que s'inclogui el _patch_ a la versió oficial, i ha publicat un _patch_ adaptat per a la versió 1.2-beta1 d'Asterisk.

Jo utilitzo la versió 1.0.9 (ultima estàble en el moment d'escriure això), així que he decidit provar el _patch_ a veure que tal funciona. Com no m'agrada compilar a mà i _embrutar_ el sistema de paquets, he _adaptat_ l'ebuild de [Gentoo](http://www.gentoo.org) per a que aplique el [_patch_ de Julian](http://www.maxosystem.net/asterisk/asterisk-stable-polarity.html). Aquí us deixo els passos a seguir per si hi ha algú més interessat.

Primer baixem el _patch_ i el posem a la carpeta adequada:

```
# cd /usr/portage/net-misc/asterisk/files/1.0.0 # wget http://www.maxosystem.net/asterisk/asterisk-stable-polarity-v5.diff
```

Seguidament <acronym title="vim asterisk-1.0.9-r1.ebuild">editem l'ebuild</acronym> i al final de la funció `src_unpack()` afegim el següent:

```
# patch for spanish reverse polarity cd ${S}/channels/ epatch ${FILESDIR}/1.0.0/asterisk-stable-polarity-v5.diff cd ${S}
```

Finalment executem la següent comanda per actualitzar el _digest_ del ebuild:

```
# ebuild asterisk-1.0.9-r1.ebuild digest
```

Després ja podem fer un `emerge asterisk` de forma normal. Recordeu modificar el `zapata.conf` per el·liminar les linies relatives a <tt>callprogress</tt>, <tt>busydetect</tt> i <tt>busycount</tt> i afegir:

```
answeronpolarityswitch=yes hanguponpolarityswitch=yes
```
