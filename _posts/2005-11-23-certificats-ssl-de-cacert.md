---
layout: post
title: Certificats SSL de CAcert
date: 2005-11-23 01:11:11.000000000 +01:00
type: post
categories:
- linux
- pofHQ
- security
tags: []
meta:
  tags: cacert ssl certificate
permalink: "/2005/11/23/certificats-ssl-de-cacert/"
---
<p><img src="{{ site.baseurl }}/assets/images/2005/11/cacert.png" alt="cacert" class="dreta border" /></p>
<p>Seguint els <a href="http://blogs.nopcode.org/brainstorm/2005/09/25/cacert-and-ssl-server-certificates/">passos</a> d'en <a href="http://blogs.nopcode.org/brainstorm/">Roman</a> he canviat el <em>self-signed certificate</em> que tenia al servidor web per un certificat signat per <a href="http://www.cacert.org/">CAcert</a>. En menys de 10 minuts he completat tots els <a href="http://www.cacert.org/help.php?id=6">passos</a> per crear-lo: et dones d'<a href="https://www.cacert.org/index.php?id=1">alta</a>,  <a href="http://www.cacert.org/help.php?id=4">generes</a> el <acronym title="Certificate Signing Request">CSR</acronym> (petició per que em signin el certificat), dones d'<a href="https://www.cacert.org/account.php?id=7">alta</a> el FQDN, <a href="https://www.cacert.org/account.php?id=10">envies</a> el CSR, i t'envien una copia signada del certificat, llesta per instal·lar-la al servidor (per exemple Apache).</p>
<p>L'script per generar el certificat, la clau privada i el CSR que jo he usat el podeu trobar <a href="http://wiki.cacert.org/wiki/VhostTaskForce#head-15f2cf5e27a280c7c16e4d82910a16871a4fb345">aquí</a>, i això és el que ens demana quan l'executem:</p>
<pre>
# ./subjectAltname.pl
Generate SSL Cert stuff for SAPI
FQDN/Keyname for Cert (ie www.example.com)              :pof.eslack.org
Alt Names (ie www1.example.com or &lt;return&gt; for none)              :lists.eslack.org
Alt Names (ie www1.example.com or &lt;return&gt; for none)              :
Host short name (ie imap big_srv etc)              :s0

Attempting openssl...
Generating a 2048 bit RSA private key
....................................+++
writing new private key to '/root/s0_privatekey.pem'
-----
writing csr to /root/s0\_csr.pem... Take the contents of /root/s0\_csr.pem and go submit them to receive an SSL ID. When you receive your public key back, you 'should' name it something like 's0\_server.pem'.

Un cop enviat el CSR a CAcert ens envien el certificat signat, amb la instal·lació d'Apache per defecte de Gentoo només cal copiar-lo a `/etc/apache2/ssl/server.crt` i també copiar la clau privada (<tt>s0_privatekey.pem</tt>) generada per l'script anterior a `/etc/apache2/ssl/server.key`. Un cop fet això reiniciem l'Apache i ja tenim un certificat signat per CAcert.

Per utilitzar el certicficat SSL de CAcert amb courier-imap-ssl i courier-pop3d-ssl, hem de fer això: (rutes per defecte a la instal·lació de Gentoo)

```
# dd if=/dev/urandom of=/tmp/randfile count=1 2\>/dev/null # cat /root/s0\_privatekey.pem /root/s0\_server.pem \> /etc/courier-imap/imapd.pem # openssl gendh -rand /tmp/randfile 512 \>\> /etc/courier-imap/imapd.pem # cat /root/s0\_privatekey.pem /root/s0\_server.pem \> /etc/courier-imap/pop3d.pem # openssl gendh -rand /tmp/randfile 512 \>\> /etc/courier-imap/pop3d.pem # rm /tmp/randfile
```

Des de CAcert s'està intentant que el _root certificate_ s'inclogui per defecte a la majoria de navegadors, [aquí](http://wiki.cacert.org/wiki/InclusionStatus) podeu veure l'estatus d'inclusions. De moment encara no està inclòs en firefox, així que si no volem que ens doni una alerta quan visitem el lloc hem d'[importar](http://wiki.cacert.org/wiki/BrowserClients) el [root certificate](http://www.cacert.org/index.php?id=3) de CAcert fent click [aquí](http://www.cacert.org/certs/root.crt).

**NOTA:** a tots els que mireu el correu a través del webmail de pofHQ us recomano que l'instal·leu.

