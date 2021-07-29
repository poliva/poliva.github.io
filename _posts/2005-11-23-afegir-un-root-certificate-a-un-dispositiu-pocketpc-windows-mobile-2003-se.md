---
layout: post
title: Afegir un root certificate a un dispositiu PocketPC (Windows Mobile 2003 SE)
date: 2005-11-23 14:10:47.000000000 +01:00
type: post
categories:
- HTC
- security
tags: []
meta:
  tags: ssl cert pda pocketpc
permalink: "/2005/11/23/afegir-un-root-certificate-a-un-dispositiu-pocketpc-windows-mobile-2003-se/"
---
Per importar el root certificate de CAcert a la PDA (per que reconegui el certificat que he generat en el [post anterior](/blog/2005/11/23/certificats-ssl-de-cacert/)) he seguit els següents passos:

- Descarregar el [root certificate de CACert en format DER](http://www.cacert.org/certs/root.der)
- Renombrar-lo a <tt>root.<strong>cer</strong></tt>
- Descarregar la utilitat [smartphoneaddcert](http://download.microsoft.com/download/0/3/b/03b3162a-c093-4434-917c-4b289d027ceb/smartphoneaddcert.exe) de Micro$oft, es un zip self executable.
- Extreure l'arxiu <tt>SPAddCert.exe</tt> del zip anterior.
- Copiar <tt>root.per</tt> a la carpeta `\Storage` de la PocketPC
- Copiar <tt>SPAddCert.exe</tt> a la PocketPC i executar-lo.

Seguidament trobarà el certificat de CAcert, l'acceptem, fem un _soft reset_ de la PDA i llestos. Per comprovar que s'ha instal·lat correctament podem anar a <tt>Settings - System - Certificates - Root</tt>.

