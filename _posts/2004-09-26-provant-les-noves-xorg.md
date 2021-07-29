---
layout: post
title: Provant les noves xorg
date: 2004-09-26 19:17:46.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: ''
permalink: "/2004/09/26/provant-les-noves-xorg/"
---
![borde sombrejat xorg-x11]({{ site.baseurl }}/assets/images/2004/09/borde-sombrejat.png)

Fa uns dies vaig provar la nova versió 6.8 de [xorg](http://www.x.org/), que permet fer coses visualment molt màques com les transparències o bordes sombrejats que podeu veure en les imatges que acompanyen a aquest post.

La primera impressió és bastant bona, però hi ha que dir que són poc usables, sobre tot si fem servir transparències el sistema es relentitza moltissim i trigues molt en arrastrar una finestra d'un lloc a un altre. Si només deixem els bordes activats la cosa canvía i funciona bastant bé, encara que de moment hi ha un [bug](http://bugs.gentoo.org/show_bug.cgi?id=64782) que fa que el firefox pete quan carregues una pàgina en flash.

Per a que funcionés tot correctament he hagut d'actualitzar el kernel, ja que en el nou 2.6.9-mm2-rc1 hi ha una nova versió del driver de la gràfica que utilitzo al portàtil, i és compatible en les xorg noves:

`CONFIG_DRM_I915:`

> Choose this option if you have a system that has Intel 830M, 845G, 852GM, 855GM 865G or 915G integrated graphics. If M is selected, the module will be called i915. AGP support is required for this driver to work. This driver will eventually replace the I830 driver, when later release of X start to use the new DDX and DRI.

![transparencia xorg-x11]({{ site.baseurl }}/assets/images/2004/09/transparencia-xorg.png)

Per a configurar les transparencies i bordes s'han d'instal·lar un parell de paquets nous, _X Compositing Manager_ i _transset_, amb Gentoo només cal fer:

```
# emerge xcompmgr # emerge transset
```

Llavors al `~/.xinitrc` s'ha d'afegir `xcompmgr -c &` i al fitxer de configuració de les X's la següent secció:

```
Section "Extensions" Option "Composite" "Enable" Option "RENDER" "Enable" EndSection
```

Un cop hem configurat tot això els bordes ja apareixeran per defecte en totes les aplicacions, per a configurar les transparències hem d'executar `transset` seguit d'un número del 0 al 1 que indica el nivell de transparencia, per defecte és 0.75. Llavors el punter del ratolí es posa en forma de creu per a que clickem a sobre de la finestra on volem aplicar la transparència sel·leccionada.

A part del bug que he comentat abans, també hi ha una altra cosa que no m'acaba d'agradar i és que com per defecte aplica bordes sombrejats a tots els programes els gdesklets -- que ja tenen els seus bordes -- queden horribles, ja que sel's hi posa un doble borde per sobre i no queden gens bé. Espero que en pròximes versions sol·lucionin aquest problema!

