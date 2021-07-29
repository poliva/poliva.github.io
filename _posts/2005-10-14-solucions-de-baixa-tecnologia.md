---
layout: post
title: Solucions de baixa tecnologia
date: 2005-10-14 01:41:06.000000000 +02:00
type: post
categories:
- gadgets
tags: []
meta:
  tags: parallel port electronics audio
permalink: "/2005/10/14/solucions-de-baixa-tecnologia/"
---
Aquesta setmana he adquirit dos nous gadgets _casolans_: un [switch de tensió](/photos/folder/albums/switch%20de%20tensio) controlat pel port paral·lel (gràcies [Lucas](http://taller.dnsalias.com)!) i un [mesclador de senyals analògiques](/photos/folder/albums/sumador%20senyals%20audio) d'àudio (gràcies [MiKi](http://mikijk.blogspot.com)!). A continuació us explico la finalitat de cada aparell.

![switch de tensió controlat pel port paral·lel]({{ site.baseurl }}/assets/images/2005/10/thumb-switchtensio.jpg)El switch de tensió agafa 12V de la font d'alimentació del PC per alimentar al trunk GSM que tinc connectat al Asterisk, des de el port paral·lel del PC puc enviar una senyal que fa commutar un relé i permet tallar la tensió (apagar el trunk) o tornar-li a donar voltatge per endollar-lo de nou. I us preguntareu quina finalitat te això? Doncs molt simple, li he posat un [adaptador dual SIM](/blog/2005/05/19/dual-sim/) al trunk, i així puc fer un canvi de SIM controlat des de port paral·lel per tenir sempre la tarifa més econòmica a trucades a mòbils.

Per a controlar l'estat dels pins del port paral·lel només cal escriure un nombre enter positiu de 8 bits a l'adreça del port. El pin de dades 0 és el bit de menys pes (pes = 1), per controlar quins pins volem activar podem seguir els valors d'aquesta taula:

| **Terminal** | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| **Bit** | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| **Pes** | 1 | 2 | 4 | 8 | 16 | 32 | 64 | 128 |

Per controlar el port paral·lel des de Linux he utilitzat [aquest programa](http://www.epanorama.net/circuits/lptout.c), l'adreça del port LPT1 és normalment 378, si volem usar el LPT2 serà 278 i el LPT3 tindrà l'adreça 3BC. ![trunk dual SIM]({{ site.baseurl }}/assets/images/2005/10/thumb-trunkdualsim.jpg)

L'altre nou gadget és un sumador de senyals analògiques d'àudio, transforma la entrada de línia de la mini-cadena (connectors RCA) en 3 entrades stereo (connectors mini jack) balancejades i en una impedància "autoajustable". Així puc connectar l'àudio de 3 PCs a la minicadena sense haver d'anar canviant de cable; la gràcia que té es que és un element completament passiu i no li fa falta alimentació per funcionar.

![sumador de senyals analògiques d'àudio]({{ site.baseurl }}/assets/images/2005/10/thumb-sumadoraudio1.jpg)

Més fotos:

- [switch de tensió controlat pel port paral·lel](/photos/folder/albums/switch%20de%20tensio)
- [mesclador de senyals analògiques d'àudio](/photos/folder/albums/sumador%20senyals%20audio)

No hi ha res com tenir amics amants de la electrònica! ;)

