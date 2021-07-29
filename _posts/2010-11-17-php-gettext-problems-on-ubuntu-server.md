---
layout: post
title: php gettext problems on ubuntu server
date: 2010-11-17 02:14:56.000000000 +01:00
type: post
categories:
- linux
- minipost
tags:
- php gettext lang
meta:
  _wp_old_slug: ''
permalink: "/2010/11/17/php-gettext-problems-on-ubuntu-server/"
---
I have gone crazy until I found this... to have gettext functions working in ubuntu server you need to add the proper locales to the system first, for example, to translate text in spanish:

`
# cat /usr/share/i18n/SUPPORTED |grep -i "es_ES" > /var/lib/locales/supported.d/es
`

And repeat for every language you need... :)

when finished, run `dpkg-reconfigure locales` and you are done!

