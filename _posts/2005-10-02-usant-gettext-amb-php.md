---
layout: post
title: Usant gettext amb php
date: 2005-10-02 22:40:28.000000000 +02:00
type: post
categories:
- linux
tags: []
meta:
  tags: php gettext i18n
permalink: "/2005/10/02/usant-gettext-amb-php/"
---
La utilitat [GNU gettext](http://www.gnu.org/software/gettext/) facilita la traducció de programes utilitzant un cataleg de missatges amb l'idioma original i la traducció. També podem utilitzar les funcions de `gettext` dintre de [php](http://www.php.net/) per a fer pàgines web amb diferents idiomes. Aquí teniu un exemple molt simple que ilustra l'ús de `gettext` amb php.

Creem una carpeta on posarem la informació dels diferents _locales_ amb una subcarpeta per a cada locale. Per exemple si volem missatges en català i en castellà crearem la següent estructura de directoris:

```
# ./locales # cd ./locales # mkdir -p es\_ES/LC\_MESSAGES/ # mkdir -p ca\_ES/LC\_MESSAGES/
```

Ara creem el cataleg de missatges per al català. Creem el fitxer `./locales/ca_ES/LC_MESSAGES/messages.po` amb el següent contingut:

```
# SOME DESCRIPTIVE TITLE. # Copyright (C) YEAR THE PACKAGE'S COPYRIGHT HOLDER # This file is distributed under the same license as the PACKAGE package. # FIRST AUTHOR , YEAR. # #, fuzzy msgid "" msgstr "" "Project-Id-Version: PACKAGE VERSION\n" "POT-Creation-Date: 2003-03-18 10:52+0400\n" "PO-Revision-Date: YEAR-MO-DA HO:MI+ZONE\n" "Last-Translator: FULL NAME \<EMAIL@ADDRESS\>\n" "Language-Team: LANGUAGE \<LL@li.org\>\n" "MIME-Version: 1.0\n" "Content-Type: text/plain; charset=UTF-8\n" "Content-Transfer-Encoding: 8bit\n" msgid "The string must be here" msgstr "La cadena ha d'anar aquí" msgid "This is a test" msgstr "Això és una prova"
```

**NOTA:** Això és una plantilla, amb valors per defecte, però per a l'exemple ja serveix. Si ho preferiu podeu utilitzar editors específics per a fitxers PO com [poedit](http://poedit.sourceforge.net/) o [KBabel](http://i18n.kde.org/tools/kbabel).

És important especificar correctament el _charset_ a l'arxiu <tt>.po</tt>, en el meu cas he utilitzat `UTF-8` per que és _charset_ que utilitzo en el servidor web. Si teniu problemes amb els accents proveu amb `ISO-8859-15`.

Un cop hem creat el PO amb els missatges que volem traduir hem de compilar-lo, per crear un arxiu <tt>.mo</tt>. Això ho farem amb la següent comanda:

```
# msgfmt -v messages.po 2 translated messages.
```

Cal tenir en compte que cada cop que canvieu el PO per afegir-hi algún missatge tindreu que recompilar el MO.

Podeu repetir el procés per al _locale_ amb castellà. Un cop finalitzat anem a utilitzar `gettext` desde `PHP`. En aquest exemple imprimirem els dos missatges que tenim al catàleg amb català, castellà i anglés.

```
\<? // Especifiquem la ubicació de les translation tables bindtextdomain("messages", "./locale/"); // Especifiquem el locale en Català setlocale(LC\_ALL, 'ca\_ES'); // Imprimim els missatges echo gettext("This is a test"); echo \_("The string must be here"); // Especifiquem el locale en Castellà setlocale(LC\_ALL, 'es\_ES'); // Imprimim els missatges echo \_("This is a test"); echo gettext("The string must be here"); // Especifiquem el locale en Anglés setlocale(LC\_ALL, 'en\_US'); // Imprimim els missatges echo gettext("This is a test"); echo \_("The string must be here"); ?\>
```

Fixeu-vos que en l'exemple he utilitzat indistintament la funció `gettext()` i la funció `_()`, que és un àlias de la primera, més còmode d'utilitzar.

Si voleu aprofundir més en aquest tema podeu consultar el [manual de gettext](http://www.gnu.org/software/gettext/manual/gettext.html) i en la [documentació de php](http://www.php.net/manual/en/ref.gettext.php).

**Update:** Tal com [comenta adobo](/blog/2005/10/02/usant-gettext-amb-php/#comment-2417) es pot generar el <tt>.po</tt> automàticament amb la comanda `xgettext -a -n --from-code=utf-8 *.php`, o amb `intltool-update`.

