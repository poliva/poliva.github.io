---
layout: post
title: Miniposts, bookmarks i clipblog
date: 2006-03-17 01:21:03.000000000 +01:00
type: post
categories:
- pofHQ
tags: []
meta:
  tags: miniposts asides wordpress plugin blog search bloglines delicious rss feed
    bookmarks clipblog
permalink: "/2006/03/17/miniposts-bookmarks-i-clipblog/"
---
Fa un temps vaig crear una nova categoria al blog per fer un _experiment_ i encara no n'havia parlat fins ara per que no tenia massa clar si li donaria ús o no, es tracta de la [categoria minipost](/blog/category/minipost/) on en el moment de publicar aquest article hi ha 8 _mini-posts_. En principi és una categoria especial, que no apareix a la portada ni als feeds (rss, atom) i la vull usar per emmagatzemar informació que em fa gràcia tenir al blog, però que no considero important com per a publicar-la a portada, simplement per que és un enllaç per recordar, una tonteria que m'ha fet gràcia o algo que m'he de mirar amb més calma però pel que sigui no he tingut temps... així que totes aquestes coses acaben a la categoria mini-post. Aquesta categoria té un [feed rss propi](feed:/blog/category/minipost/rss2) on us podeu sindicar si voleu llegir els meus miniposts.

També he afegit dues seccions noves: Bookmarks i ClipBlog. La secció [Bookmarks](/blog/category/bookmarks/) és un mirror del [meu del.icio.us](http://del.icio.us/pof), i la secció [ClipBlog](/blog/category/clipblog/) és un mirror del [ClipBlog de Bloglines](http://www.bloglines.com/blog/pof) on guardo els posts que llegeixo a altres blogs que considero interessants. Aquestes seccions també tenen un rss propi on us podeu sindicar: [Bookmarks rss](feed:/blog/category/bookmarks/rss) i [ClipBlog rss](feed:/blog/category/clipblog/rss2)

**Perquè he fet això?** Doncs perquè així des de la capsa de cerca del meu blog tinc un lloc unificat per buscar també als enllaços que publico a [del.icio.us](http://del.icio.us) i als clips que faig a [bloglines](http://www.bloglines.com). La idea inicial va sorgir perquè bloglines al teu clipblog només et mostra els últims 20 clips, i no permet fer cerques als clips antics (molt lleig per un site que pertany a un cercador, [Ask Jeeves](http://www.ask.com/)) i això és molt incomode perquè sempre busco algo que he clipejat fa temps i mai ho trobo. Un cop muntat per bloglines vaig pensar que no costaria molt adaptar-ho també per del.icio.us, i així ha quedat.

**Com ho he fet?** Doncs utilitzant 4 plugins per wordpress i tocant una mica el codi per integrar-ho tot. Els plugins son aquests:

- [MiniPosts](http://doocy.net/mini-posts/) per fer la categoria miniposts, i fer que els miniposts, els bookmarks i el clipblog no apareguin ni als feeds ni a la portada.
- [FeedWordpress](http://projects.radgeek.com/feedwordpress), per a importar els RSSs de delicious i bloglines, l'he modificat per a que quan importi els RSS els faci directament miniposts, així apareixen ocults. (hint: es podria fer el mateix amb flickr o qualsevol altre site que tingui feed)
- [Search Everything](http://dancameron.org/searcheverything/), aquest ja l'utilitzava (permet buscar dintre de comentaris i pàgines estàtiques) i l'he modificat per a que també busqui dintre dels posts ocults, ja que al filtrar-los del loop el wordpress els excloïa de les cerques.
- Finalment [Custom Posts Per Page](http://wordpress.org/support/6/11211) per a augmentar el nombre de enllaços que es mostren a les categories de 10 (per defecte) a 25 que és el valor que he deixat de moment.

Si voleu baixar-vos els plugins amb les meves modificacions per integrar-ho al vostre blog els he empaquetat tots [aquí](/archives/files/plugins.tar.gz), que aprofiti :).

