---
layout: post
title: Baixa't els podcasts de Bloglines amb blogenc
date: 2004-11-08 04:28:34.000000000 +01:00
type: post
categories:
- linux
tags: []
meta:
permalink: "/2004/11/08/baixat-els-podcasts-de-bloglines-amb-blogenc/"
---
Avui he provat [blogenc](http://www.cantoni.org/projects/blogenc.html), un script en perl que permet baixar-se els [podcasts](http://en.wikipedia.org/wiki/Podcast) en mp3 als que estem subscrits amb [bloglines](http://www.bloglines.com), per a passar-los després a la PDA2k i poder escoltar-los en alguna estona morta al metro o al tren.

Aquest script utilitza els [Bloglines Web Services](http://www.bloglines.com/services) i per tan necessita el mòdul de perl pertinent, que no està a portage de moment.

Tal com he comentat en el [post anterior](/blog/2004/11/08/105/), amb `g-cpan.pl`, instal·lar-lo és tan senzill com fer això:

· Instal·lem la primera dependència, que tenim a portage:

```
# emerge MP3-Tag
```

· Seguidament instal·lem les dependències que no estàn a portage:

```
# g-cpan.pl WebService::Bloglines [...] \>\>\> dev-perl/WebService-Bloglines-0.02 merged. \>\>\> Recording dev-perl/WebService-Bloglines in "world" favorites file... # g-cpan.pl Log::Log4perl [...] \>\>\> dev-perl/Log-Log4perl-0.48 merged. \>\>\> Recording dev-perl/Log-Log4perl in "world" favorites file...
```

NOTA: Si us dona un error el `Log::Log4perl`, elimineu la última dependencia (on posa "_dev-perl/_) del ebuild autogenerat amb `g-cpan.pl`, al fitxer `/usr/local/portage/dev-perl/PathTools/PathTools-3.01.ebuild`.

Utilitzar-lo és ben senzill, editem l'arxiu blogenc.pl per dir-li el nostre login i password, i la carpeta on tenim els feeds amb podcasts i seguidament l'executem:

```
pau@cool[]~/$ ./blogenc.pl 2004/11/08 03:16:14 Script start (blogenc v0.10) 2004/11/08 03:16:14 Logging in to Bloglines as user pau@eslack.org 2004/11/08 03:16:14 Reading list of feeds and folders 2004/11/08 03:16:17 Found specified folder [podcast] 2004/11/08 03:16:17 Engadget Podcast has 1 unread items 2004/11/08 03:16:17 Fetching feed Engadget Podcast 2004/11/08 03:16:18 Got the feed, now scanning for enclosures 2004/11/08 03:16:18 Feed has 1 enclosures 2004/11/08 03:16:18 Downloading Engadget\_Podcast11.mp3 2004/11/08 03:18:07 Successful download 2004/11/08 03:18:07 msmobiles.com Podcast has 0 unread items 2004/11/08 03:18:07 The Wireless Weblog: Podcast has 0 unread items 2004/11/08 03:18:07 Creating M3U playlist Podcast 11-08 0316.m3u 2004/11/08 03:18:07 Script complete pau@cool[]~/$
```
