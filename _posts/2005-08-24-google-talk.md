---
layout: post
title: Google Talk
date: 2005-08-24 07:13:15.000000000 +02:00
type: post
categories:
- voip
tags: []
meta:
  tags: google talk IM jabber xmpp voip
permalink: "/2005/08/24/google-talk/"
---
<p><img src="{{ site.baseurl }}/assets/images/2005/08/googletalk.png" alt="Google Talk" class="left" />Ja està disponible <a href="http://www.google.com/talk/">Google Talk</a>, un servei de missatgeria instantània amb suport de trucades de veu (VoIP) de <a href="http://www.google.com/">Google</a> que funciona amb el login i password de <a href="http://gmail.google.com/">GMail</a> i utilitza el protocol <acronym title="Extensible_Messaging_and_Presence_Protocol">XMPP</acronym> (Jabber), per tant és possible connectar des de qualsevol client que suporti aquest protocol sobre SSL (<a href="http://gaim.sf.net/">Gaim</a>, <a href="http://psi.affinix.com/">PSI</a> ...).</p>
<p>De moment només és possible fer trucades de veu utilitzant el client que proporciona Google --- només disponible per a windows ---
però si llegim la [developer info](http://www.google.com/talk/developer.html) ens dona algunes pistes que tenen molt bona pinta:

> **What protocols are used for voice calls?**
> 
> Google Talk supports a custom XMPP-based signaling protocol and peer-to-peer communication mechanism. We will fully document this protocol. In the near future, we plan to support SIP signaling.
> 
> **Which voice codecs do you support?**
> 
> Today, Google Talk supports the following standard voice codecs: PCMA, PCMU, G.723, iLBC. We are also evaluating the Speex codec. We also support codecs from Global IP Sound: ISAC, IPCMWB, EG711U, EG711A

Què vol dir això? Doncs gràcies a que documentaran el protocol de veu que utilitza el seu client sobre <acronym title="Extensible_Messaging_and_Presence_Protocol">XMPP</acronym> segurament no trigarem en veure clients per a Linux que l'implementen, i ja estic pensant en un _transport_ per a interconnectar-lo amb [asterisk](http://www.asterisk.org/) i poder fer trucades a la pstn... estarem a la espera ;)

