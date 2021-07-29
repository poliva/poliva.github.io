---
layout: post
title: Interconnectar dos asterisks amb IAX
date: 2005-06-25 21:22:01.000000000 +02:00
type: post
categories:
- voip
tags: []
meta:
  tags: asterisk voip iax
permalink: "/2005/06/25/interconnectar-dos-asterisks-amb-iax/"
---
Avui hem interconnectat l'asterisk de casa amb el de l'[Esteve](http://esteve.tizos.net). La configuració és bastant senzilla, aquí teniu el nostre exemple. Cal tenir en compte que s'ha de canviar _my\_secret_ pel password que desitgeu i _pof.eslack.org_ i _tizos.net_ per les IP's dels asterisks que vulgueu interconnectar.

**Servidor 1 (pofhq):**  
`/etc/asterisk/iax.conf`:

```
; Connectar al asterisk de l'esteve per IAX [tizos] username=tizos host=dynamic type=friend secret=my\_secret context=default auth=plaintext register =\> tizos:my\_secret@tizos.net:4569 ; Permetre que esteve es connecti a mi [eslack] username=eslack host=dynamic type=user secret=my\_secret context=default auth=plaintext
```

`/etc/asterisk/extensions.conf`:

```
[tizos-out] exten =\> \_159.,1,Dial(IAX2/tizos:my\_secret@tizos.net/${EXTEN:3},60,r) exten =\> \_159.,2,Hangup
```

S'ha d'incloure `tizos-out` al context _default_, o al que fasi servir l'usuari que volem que pugui trucar cap a l'altre asterisk.

**Servidor 2 (tizos):**  
`/etc/asterisk/iax.conf`:

```
; Connectar al asterisk del pof per IAX [eslack] username=eslack host=dynamic type=friend secret=my\_secret context=default auth=plaintext register =\> eslack:my\_secret@pof.eslack.org:4569 ; Permetre que pof es connecti a mi [tizos] username=tizos host=dynamic type=user secret=my\_secret context=default auth=plaintext
```

`/etc/asterisk/extensions.conf`:

```
[eslack-out] exten =\> \_159.,1,Dial(IAX2/eslack:my\_secret@pof.eslack.org/${EXTEN:3},60,r) exten =\> \_159.,2,Hangup
```

S'ha d'incloure `eslack-out` al context _default_, o al que fasi servir l'usuari que volem que pugui trucar cap a l'altre asterisk.

Finalment, per trucar d'un asterisk a l'altre s'ha de marcar `159+numero`, on `numero` és el número de la extensió interna on volem trucar. Podeu canviar el 159 a la configuració pel número que més us agradi.

