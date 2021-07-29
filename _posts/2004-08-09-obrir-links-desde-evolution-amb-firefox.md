---
layout: post
title: Obrir links desde evolution amb Firefox
date: 2004-08-09 08:16:11.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: ''
permalink: "/2004/08/09/obrir-links-desde-evolution-amb-firefox/"
---
Per fi he descobert com fer que per a quan facis _click_ a un enllaç dintre d'un correu al evolution l'enllaç s'obri correctament amb el Firefox, ho penjo aquí per no oblidar-me'n mai més, perque la comanda té tela:

```
gconftool-2 --set /desktop/gnome/url-handlers/http/command -t string '/path/to/launcher.sh %s'
```

i el contingut del script `launcher.sh`:

```
#!/bin/sh gnome-moz-remote --remote='openURL('$1', new-tab)' || firefox $1
```

d'aquesta manera obre la web en un nou tab si el firefox està executant-se, o el llança de nou en el cas de que encara no ho estigui.

