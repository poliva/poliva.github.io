---
layout: post
title: Usar vim per llegir pàgines man i info
date: 2005-09-13 00:57:02.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: vim man info linux shell
permalink: "/2005/09/13/usar-vim-per-llegir-pagines-man-i-info/"
---
![vim]({{ site.baseurl }}/assets/images/2005/09/vim48x48.png)Un truc genial que he aprés gràcies a <acronym title="Gentoo Weekly News"><a href="http://www.gentoo.org/news/en/gwn/20050725-newsletter.xml#doc_chap5">GWN</a></acronym> consisteix en utilitzar <tt>vim</tt> per a llegir les pàgines <tt>man</tt>, per fer-ho només cal executar les següents comandes que modificaran el `~/.vimrc` i el `~/.bashrc`:

```
$ echo 'runtime ftplugin/man.vim' \>\> ~/.vimrc $ echo 'function vimman () { vim -R -c "Man $1 $2" -c "bdelete 1"; }' \>\> ~/.bashrc $ echo 'alias man=vimman' \>\> ~/.bashrc
```

Dins del _man-page browser_ de <tt>vim</tt> podem usar `CTRL-]` per a fer un <tt>man</tt> de la paraula sota el cursor, i `CTRL-T` per tornar enrera. Més informació disponible amb `:help Man`.

Per a les pàgines <tt>info</tt>, necessitem fer un `emerge app-vim/info` (Gentoo), i després executar les següents comandes:

```
$ echo 'function viminfo () { vim -R -c "Info $1 $2" -c "bdelete 1"; }' \>\> ~/.bashrc $ echo 'alias info=viminfo' \>\> ~/.bashrc
```

Si premem la tecla `H` dins d'una pàgina <tt>info</tt> mostrada amb <tt>vim</tt> veurem també una petita ajuda.

Si necessitem les comandes <tt>man</tt> o <tt>info</tt> originals, podem aconseguir-les igualment executant `\man` o `\info`.

