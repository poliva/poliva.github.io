---
layout: post
title: 'Free World DialOUT: Com trucar gratis a qualsevol lloc del món'
date: 2005-06-13 23:22:04.000000000 +02:00
type: post
categories:
- voip
tags: []
meta:
  enclosure: |-
    /blog/wp-content/tux_phone.gif
    36659
    image/gif
  tags: ''
permalink: "/2005/06/13/free-world-dialout-com-trucar-gratis-a-qualsevol-lloc-del-mon/"
---
![tux phone]({{ site.baseurl }}/assets/images/2005/06/tux_phone.png)A continuació us explico en que consisteix i com es configura el servei de [Free World DialOUT (fwdOUT)](http://www.fwdout.net/), una xarxa d'intercanvi de trucades telefòniques que permet fer trucades de forma gratuïta a altres parts del món. El lema de fwdOUT és _The love you take is equal to the love you make_, això vol dir que només podrem fer trucades gratis si nosaltres deixem que altres usuaris en facin a través de nosaltres... i com ho fem? doncs amb totes aquestes noves tarifes que estan oferint ara gaire bé totes les operadores de telefonia fixa convencional, on per un mòdic preu ens permeten fer trucades nacionals gratis a números fixes.

Què necessitem?

1. Un [asterisk](/blog/2005/04/21/asterisk-the-open-source-linux-pbx/)
2. Una [targeta FXO](/blog/2005/05/08/configuracio-de-la-tarjeta-x100p-amb-asterisk/)
3. Una línia de telèfon convencional amb [tarifa plana de trucades](/blog/2005/06/10/connexio-a-2mbps/)
4. Donar-nos d'[alta a fwdOUT](http://www.fwdout.net/bell-cgi/signup.cgi)

Un cop ens hem donat d'alta, hem de configurar el nostre servidor asterisk tal com veureu a continuació.

Al fitxer `/etc/asterisk/iax.conf`, dintre de la secció `general` afegim el següent:

```
[general] register =\> 12345:mypassword@iax.fwdOUT.net allow=ulaw allow=gsm
```

Al mateix fitxer, afegirem una nova secció `fwdOUT`:

```
[fwdOUT] type=friend username=12345 secret=mypassword host=iax.fwdOUT.net context=fromfwdOUT auth=rsa inkeys=freeworlddialup
```

Heu d'especificar el vostre número d'usuari i el password corresponent.

Al fitxer `/etc/asterisk/extensions.conf` configurarem una extensió per trucar a través de fwdOUT, això ho fem dins del context per defecte. En el meu cas he triat la extensió 156.

```
exten =\> \_156.,1,SetCallerId,yourUser exten =\> \_156.,2,Dial(IAX2/12345@fwdOUT/${EXTEN:3},60,r) exten =\> \_156.,3,Congestion
```

Seguidament definirem un context on arribaran les trucades provinents de fwdOUT i sortiràn per la línia telefònica a través de la targeta FXO. Fixeu-vos en alguns detalls d'aquesta configuració:

1. Limito les trucades a 30 minuts com a màxim.
2. Marco el número amb un 067 al davant, per ocultar el CallerID.
3. Em menjo els dos primers números de la extensió que m'arriba (el 34) perquè si no la operadora ens informarà de que el número no existeix. (Això és el :2 que hi ha després de la variable EXTEN).
4. La Z (\_349ZXXXXXXX) és qualsevol número del 1 al 9, que no sigui 0, per evitar trucades a 902, 908...
5. També permeto trucar als números gratuïts (prefixe 900).

```
[fromfwdOUT] ;match the context in iax.conf ; tots els fixes (349...) excepte els 90X, ej 908 902... exten =\> \_349ZXXXXXXX,1,Dial(ZAP/1/067${EXTEN:2},60,T,L(1800000:1790000)) ; tots els 900, son num. gratuïts... exten =\> \_34900XXXXXX,1,Dial(ZAP/1/067${EXTEN:2},60,T,L(1800000:1790000))
```

A continuació definim un context per capturar les trucades que entren dintre de la ruta que publiquem però que no volem que surtin per la nostra línia (les trucades a +34908 per exemple):

```
include =\> fromfwdOUT-catchall [fromfwdOUT-catchall] exten =\> \_.,1,Congestion exten =\> h,1,Hangup ;hangup event exten =\> i,1,Hangup ;invalid event exten =\> t,1,Hangup ;timeout event
```

Finalment publiquem la ruta a través del apartat _[My Routes](http://www.fwdout.net/bell-cgi/fwdOUT.cgi?mode=My_Routes)_ de fwdOUT. Aquesta és la meva ruta que permet arribar als números que comencen per 34+9 (codi d'espanya + prefix de telèfons fixes):

![fwdout route]({{ site.baseurl }}/assets/images/2005/06/rutafwd.png)

Des-d'aquest apartat podem definir quantes trucades volem permetre per hora, i les trucades restants de cada dia.

I això és tot, ja hem convertit el nostre asterisk en un gateway entre la xarxa telefònica convencional i la xarxa de fwdOUT. Ara només cal esperar a que algú fassi trucades a fixes espanyols a través de la nostra ruta per a que ens aumentin els crèdits disponibles i ja podrém fer trucades a qualsevol lloc del món, sempre i quan hi hagi algú connectat a la xarxa de Free World DialOUT per fer arribar la nostra trucada al destí.

