---
layout: post
title: Asterisk dialplan, pas a pas
date: 2005-09-14 01:49:52.000000000 +02:00
type: post
categories:
- linux
- voip
tags: []
meta:
  tags: VoIP asterisk dialplan SIP IAX extensions FWD FWDOUT x100p movistar menta
    auna voipjet voipuser voipbuster Adam voztelecom telephony
permalink: "/2005/09/14/asterisk-dialplan-pas-a-pas/"
---
<p>En aquest post us comentaré com tinc muntat el <em>dialplan</em> al <a href="http://www.asterisk.org/">Asterisk</a> de casa. Pot servir com a exemple de qualsevol instal·lació de VoIP domèstica: una màquina linux amb Asterisk, dues <a href="/blog/2005/04/21/x100p-oem-fxo-pci-card/">targetes x100p</a> una connectada a una línia convencional (<a href="http://www.aunacable.es">menta</a>) amb trucades nacionals gratuïtes, i l'altra connectada a un <a href="http://www.audiotel.it/EN/PRODOTTI/prodotti.asp?fam=1&amp;prod=5">trunk GSM</a> amb un mòbil (<a href="http://www.movistar.es">movistar</a>) de prepagament.</p>
<p>A part d'això tenim varies opcions per fer trucades per VoIP, que resumeixo a continuació:</p>
<ul>
<li>
<p><a href="http://www.voipjet.com/">VoipJet</a>: El proveïdor amb terminació <acronym title="Inter Asterisk eXchange">IAX</acronym> més <a href="http://www.voipjet.com/prices.php">econòmic</a> que hem trobat.</p>
</li>
<li>
<p><a href="http://www.voipbuster.com">VoipBuster</a>: Un altre proveïdor amb terminació <acronym title="Inter Asterisk eXchange">IAX</acronym>, una mica <a href="http://www.voipbuster.com/en/rates.html">més car</a> que l'anterior però que permet trucar gratis a fixes d'Espanya i varis països més.</p>
</li>
<li>
<p><a href="http://www.voipuser.org/">VoIP User</a>: Proveïdor amb terminació <acronym title="Session Initiation Protocol">SIP</acronym> que ofereix <a href="http://www.voipuser.org/forum_post_1397.html#1397">trucades gratuïtes</a> a fixes de varis països.</p>
</li>
<li>
<p><a href="http://www.freeworlddialup.com/">Free World DialUP</a>: Proveïdor amb terminació <acronym title="Inter Asterisk eXchange">IAX</acronym> que permet trucar gratuïtament a altres membres d'aquest proveïdor, a clients de <a href="http://www.voztele.com/">Voz Telecom</a> i a telèfons <em>toll-free</em> de varis països.</p>
</li>
<li>
<p><a href="http://www.fwdout.net/">Free World DialOut</a>: Xarxa d'intercanvi de trucades amb terminació <acronym title="Inter Asterisk eXchange">IAX</acronym>. Permet fer trucades gratuïtes a altres països, a canvi de que nosaltres deixem que gent de fora les faci per la nostra línia (nacionals a través de menta que ens surt gratis).</p>
</li>
<li>
<p><a href="http://vozip.adam.es">Adam</a>: Proveïdor espanyol amb terminació <acronym title="Session Initiation Protocol">SIP</acronym> que ofereix trucades gratuïtes entre els usuaris d'aquest proveïdor i <a href="http://vozip.adam.es/index.php?option=com_content&amp;task=view&amp;id=24&amp;Itemid=48">altres xarxes</a>.</p>
</li>
</ul>
<p><!--more--></p>
<h3>Introducció</h3>
<p>El <em>dialplan</em> es configura al fitxer <code>/etc/asterisk/extensions.conf</code>. Primer definim una serie de paràmetres de configuració generals i variables globals. Fixeu-vos que al Asterisk tinc 3 usuaris (l'<a href="http://esteve.tizos.net">Esteve</a>, el <a href="http://www.minid.net">Diego</a> i jo), i que compartim el mateix <em>account</em> als proveïdors gratuïts, però tenim comptes separats per als proveïdors de pagament, així cadascú controla el que gasta per separat.</p>
<pre>
[general]
static=yes
writeprotect=yes

[globals]
; Some global vars
POFMOV=656xxxxxx
ESTEVEMOV=636xxxxxx
DIEGOMOV=677xxxxxx
; VoipJet accounts
POFVOIPJET=xxxx
ESTEVEVOIPJET=xxxx
DIEGOVOIPJET=xxxx
; Free World DialOUT
POFFWDOUT=xxxx
POFMAIL=pau@xxxxxx.xxx
; Free World DialUP Settings
FWDNUMBER=646499                                ; your calling number
FWDCIDNAME=646499                               ; your caller id
FWDPASSWORD=xxxx                                ; your password
FWDRINGS=sip/200                                ; the phone to ring
FWDVMBOX=200                                    ; the VM box for this user
</pre>
<p>Evidentment les <tt>xxxx</tt> van substituïdes pels corresponents números de telèfon o passwords. Això només son variables, per no haver d'escriure cada cop el meu telèfon, posteriorment utilitzo <code>${POFMOV}</code>.</p>
<h3>Macros</h3>
<p>A continuació defineixo unes macros que utilitzaré també posteriorment en el fitxer. Les macros són com funcions dins d'un programa i els podem passar tants arguments com vulguem quan les cridem, dintre de la macro es substitueixen les variables ${ARG1}, ${ARG2}, etc... pel valor del paràmetre que hem passat quan hem cridat a la macro.</p>
<h4>Macro mactollfree</h4>
<p>L'objectiu de la següent macro és trucar al número que se li passi com a primer paràmetre per <acronym title="Free World DialUP">FWD</acronym> i en cas de que no es pugui, trucar per <acronym title="Free World DialOut">FWDOUT</acronym>.</p>
<pre>
;--------------------------------------------
; macros
;--------------------------------------------

[macro-mactollfree]
;   ${ARG1} - Extension  (we could have used ${MACRO_EXTEN} here as well
;        Truco amb FreeWorldDialUp (s'ha de posar un * davant)
;        si no va, truco amb FreeWorldDialOut (perdo credits)
exten =&gt; s,1,SetCallerID,${FWDCIDNAME}
exten =&gt; s,2,Playback(pof-fwdup)
exten =&gt; s,3,Dial(IAX2/${FWDNUMBER}@fwd-gw/*${ARG1},60,g)
exten =&gt; s,4,NoOp(Contestat:${ANSWEREDTIME})
exten =&gt; s,5,GotoIf($["${ANSWEREDTIME}" = ""]?6:9)
exten =&gt; s,6,SetCallerID,${POFMAIL}
exten =&gt; s,7,Playback(pof-fwdout)
exten =&gt; s,8,Dial(IAX2/${POFFWDOUT}@fwdOUT/q${ARG1},60)
exten =&gt; s,9,Hangup
</pre>
<p>Analitzem la macro pas per pas: Aquesta macro es crida posteriorment, de la següent manera:</p>
<p><code>exten =&gt; _1800NXXXXXX,1,Macro(mactollfree,${EXTEN})</code></p>
<p>Que <em>traduït</em> per als que no estigueu familiaritzats amb la sintaxi d'asterisk vol dir això: quan el numero marcat començi per <code>1800</code> (és com el prefixe 900 d'aquí però a USA, son números gratuïts), seguit d'un nombre del 2 al 9 (la <code>N</code>), seguit de 6 nombres del 0 al 9 (les 6 <code>X</code>'s) executa la <code>Macro <tt>mactollfree</tt></code> passant-li com a paràmetre 1 la extensió completa, es a dir, el número marcat.</p>
<p>Llavors la macro executarà la seqüència de comandes que conté:</p>
<ol>
<li>
<p>Posa't com a CallerID el meu número de <acronym title="Free World DialUp">FWD</acronym>, emmagatzemat en la variable FWDCIDNAME que hem definit al principi del fitxer.</p>
</li>
<li>
<p>Reprodueix el fitxer <tt>pof-fwdup</tt>, que és un fitxer d'àudio que m'informa de que la trucada s'enrutarà a través de FWDUP.</p>
</li>
<li>
<p>Executa la comanda <tt>Dial</tt> que és la que fa efectiva la trucada, fixeu-vos que li passem 3 paràmetres separats per comes:</p>
</li>
<ul>
<li><code>IAX2/${FWDNUMBER}@fwd-gw/*${ARG1}</code>: Trucada a través del protocol <acronym title="Inter Asterisk eXchange">IAX2</acronym> de 646499 (el valor de la variable FWDNUMBER definit al principi) a través de <tt>fwd-gw</tt> (definit a <code>/etc/asterisk/iax.conf</code>) cap a <code>*${ARG1}</code> que és la extensió marcada (el número 1-800) amb un asterisk al davant, ja que amb <acronym title="Free World DialUp">FWD</acronym> s'ha de <a href="http://www.freeworlddialup.com/advanced/peering_numbers">marcar un *</a> al davant dels números <em>toll-free</em>.</li>
<li><code>60</code>: 60 segons de <em>timeout</em> de trucada, si en aquest temps no contesten acaba la execució de <tt>Dial</tt>.</li>
<li><code>g</code>: Opció que indica a <tt>Dial</tt> que quan finalitzi la execució, continui executant més comandes en el context actual, és a dir que no finalitzi aquí sinó que segueixi. Aquesta opció és important, ja que és la que ens permet fer que si la trucada no s'enruta correctament a través de <acronym title="Free World DialUP">FWD</acronym> continui executant més comandes i truqui utilitzant <acronym title="Free World DialOUT">FWDOUT</acronym>, un altre proveïdor que també ofereix trucades gratuïtes a números <em>toll-free</em>.</li>
</ul>
<li>
<p>El <tt>NoOp</tt> és una comanda que indica a Asterisk que no faci rés, bé... casi res, la utilitzem com a <em>printf</em> o <em>echo</em> per a que ens imprimeixi el valor de la variable ${ANSWEREDTIME} per la consola d'Asterisk. Només és útil per <em>debugar</em>.</p>
</li>
<li>
<p>Després de la execució de <tt>Dial</tt> la variable ANSWEREDTIME conté el temps que s'ha contestat la trucada, en el cas de que aquesta hagi estat contestada, sinó no conté res. El que fem en aquesta línia és dir: Si ANSWEREDTIME no té valor salta a la línia 6 (com no s'ha pogut efectuar la trucada per <acronym title="Free World DialUP">FWD</acronym> saltem al següent proveïdor), en cas contrari (ANSWEREDTIME té algun valor) saltem a la línia 9 on farem un <tt>Hangup</tt> perquè la trucada ha finalitzat correctament.</p>
</li>
<li>
<p>Si estem en aquesta línia vol dir que la trucada no ha anat bé per <acronym title="Free World DialUP">FWD</acronym>, per tant la enrutarem a través de <acronym title="Free World DialOUT">FWDOUT</acronym>, que vol que li passem una adreça de mail com a CallerID. Això és el que fem aquí.</p>
</li>
<li>
<p>Igual que abans, reproduïm un fitxer d'àudio informant a la persona que truca a través de quin proveïdor s'enrutarà la trucada.</p>
</li>
<li>
<p>Executem la comanda <tt>Dial</tt> que fa efectiva la trucada, els paràmetres que li passem són molt semblants als que passàvem anteriorment, només comentar que aquí la <code>q</code> que posem davant de la extensió serveix per a que <a href="http://www.fwdout.net/cgi-bin/twiki/view/Main/FaQ#Can_I_bypass_the_announcements_"><acronym title="Free World DialOUT">FWDOUT</acronym> no reprodueixi la locució</a> informant dels credits que et queden. Fixeu-vos que aquí ja no utilitzem la <code>g</code> com a paràmetre, ja que no hem de seguir executant comandes en el context després d'aquest <tt>Dial</tt>.</p>
</li>
<li>
<p>Per finalitzar, penjem la trucada.</p>
</li>
</ol>
<h4>Macro macvoipjet</h4>
<p>A continuació tinc una altra macro que utilitzo per a trucar a través de <a href="http://www.voipjet.com">VoipJet</a>. Aquesta macro té dues coses importants:</p>
<ol>
<li>
<p>Utilitza els 3 servidors de VoipJet, així si el primer està caigut salta al segon... i sinó al tercer. Per fer això utilitzo el mateix mètode que he descrit abans, comprovant el valor de ANSWEREDTIME.</p>
</li>
<li>
<p>Envia correctament el número a VoipJet sense que l'usuari s'hagi de preocupar: 011 davant dels números internacionals, i sense 011 als números del <acronym title="North American Numbering Plan Administration">NANPA</acronym>.</p>
</li>
</ol>
<pre>
[macro-macvoipjet]
;   ${ARG1} - Extension
;   ${ARG2} - CallerID  (x.ej: ${POFMOV})
;   ${ARG3} - Account   (x.ej: ${POFVOIPJET})
;   ${ARG4} - Server1   (x.ej: voipjet-pau-out)
;   ${ARG5} - Server2   (x.ej: voipjet-pau-out2)
;   ${ARG6} - Server3   (x.ej: voipjet-pau-out3)
; Faig trucades pels 3 servidors de voipjet, en cas de q un estigui caigut
; NANPA: North American Numbers dialed as 1 + area code
exten =&gt; s,1,SetCallerID(0${ARG2})
exten =&gt; s,2,Playback(pof-voipjet)
exten =&gt; s,3,GotoIf($["${ARG1:0:4}" = "0111"]?4:12)
exten =&gt; s,4,Dial(IAX2/${ARG3}@${ARG4}/${ARG1:3},60,g)
exten =&gt; s,5,NoOp(Contestat:${ANSWEREDTIME})
exten =&gt; s,6,GotoIf($["${ANSWEREDTIME}" = ""]?7:20)
exten =&gt; s,7,Dial(IAX2/${ARG3}@${ARG5}/${ARG1:3},60,g)
exten =&gt; s,8,NoOp(Contestat:${ANSWEREDTIME})
exten =&gt; s,9,GotoIf($["${ANSWEREDTIME}" = ""]?10:20)
exten =&gt; s,10,Dial(IAX2/${ARG3}@${ARG6}/${ARG1:3},60)
exten =&gt; s,11,Hangup
; WORLD: International Numbers dialed as 011 + country code + number
exten =&gt; s,12,GotoIf($["${ARG1:0:3}" = "011"]?13:20)
exten =&gt; s,13,Dial(IAX2/${ARG3}@${ARG4}/011${ARG1:3},60,g)
exten =&gt; s,14,NoOp(Contestat:${ANSWEREDTIME})
exten =&gt; s,15,GotoIf($["${ANSWEREDTIME}" = ""]?16:20)
exten =&gt; s,16,Dial(IAX2/${ARG3}@${ARG5}/011${ARG1:3},60,g)
exten =&gt; s,17,NoOp(Contestat:${ANSWEREDTIME})
exten =&gt; s,18,GotoIf($["${ANSWEREDTIME}" = ""]?19:20)
exten =&gt; s,19,Dial(IAX2/${ARG3}@${ARG6}/011${ARG1:3},60)
exten =&gt; s,20,Hangup
</pre>
<p>Si heu entès la macro <tt>mactollfree</tt> que he comentat anteriorment no us costarà entendre aquesta, així que comentaré només els punts que considero més "<em>difícils de desxifrar</em>", si a algú li interessa i no entén alguna cosa podeu preguntar als comentaris.</p>
<ul>
<li>Al SetCallerID poso un 0 davant, per que VoipJet obliga a que el CallerID tingui 10 dígits.</li>
<li>Sempre crido a la macro amb 011 davant de la extensió (número a trucar), i a la macro comprovo si te 0111 és un <acronym title="North American Numbering Plan Administration">NANPA</acronym>, sinó és un número d'un altre país.</li>
<li>Als <acronym title="North American Numbering Plan Administration">NANPA</acronym> els extrec el 011 de davant amb el <code>:3</code> que hi ha al final de la extensió: <code>${ARG1:3}</code>.</li>
<li>Als que no són <acronym title="North American Numbering Plan Administration">NANPA</acronym> també els extrec els 3 primers dígits (és una tonteria, no caldria fer-ho) però els hi poso fixes abans de la variable: <code>/011${ARG1:3}</code>.</li>
</ul>
<h4>Macro macpof-dialout</h4>
<p>A continuació una altra macro, molt semblant a la anterior, però que afegeix un nou proveïdor: <a href="http://www.voipbuster.com">VoipBuster</a>. Aquí el que faig és que en cas de que la trucada sigui a Estats Units la envio per VoipBuster, perquè té tarifes més econòmiques que VoipJet. Si VoipBuster estigués caigut, provaria els 3 servidors de VoipJet. Si la trucada no és a USA, llavors la envio per VoipJet i si els 3 servidors de VoipJet estiguessin caiguts sortiria per VoipBuster.</p>
<pre>
[macro-macpof-dialout]
;   ${ARG1} - Extension
;   ${ARG2} - CallerID  (x.ej: ${POFMOV})
;   ${ARG3} - Account   (x.ej: ${POFVOIPJET})
;   ${ARG4} - Server1   (x.ej: voipjet-pau-out)
;   ${ARG5} - Server2   (x.ej: voipjet-pau-out2)
;   ${ARG6} - Server3   (x.ej: voipjet-pau-out3)
; Faig trucades pels 3 servidors de voipjet, en cas de q un estigui caigut
; si els 3 servers de voipjet estan caiguts, surto per voipbuster
; NANPA: North American Numbers dialed as 1 + area code
; usa calls are cheaper with voipbuster
exten =&gt; s,1,SetCallerID(0${ARG2})
exten =&gt; s,2,GotoIf($["${ARG1:0:4}" = "0111"]?3:16)
exten =&gt; s,3,Playback(pof-voipbuster)
exten =&gt; s,4,Dial(IAX2/pauoliva@voipbuster-pau-out/00${ARG1:3},60,g)
exten =&gt; s,5,NoOp(Contestat:${ANSWEREDTIME})
exten =&gt; s,6,GotoIf($["${ANSWEREDTIME}" = ""]?7:29)
exten =&gt; s,7,Playback(pof-voipjet)
exten =&gt; s,8,Dial(IAX2/${ARG3}@${ARG5}/${ARG1:3},60,g)
exten =&gt; s,9,NoOp(Contestat:${ANSWEREDTIME})
exten =&gt; s,10,GotoIf($["${ANSWEREDTIME}" = ""]?11:29)
exten =&gt; s,11,Dial(IAX2/${ARG3}@${ARG6}/${ARG1:3},60,g)
exten =&gt; s,12,NoOp(Contestat:${ANSWEREDTIME})
exten =&gt; s,13,GotoIf($["${ANSWEREDTIME}" = ""]?14:29)
exten =&gt; s,14,Dial(IAX2/${ARG3}@${ARG4}/${ARG1:3},60)
exten =&gt; s,15,Hangup
; WORLD: International Numbers dialed as 011 + country code + number
exten =&gt; s,16,GotoIf($["${ARG1:0:3}" = "011"]?17:29)
exten =&gt; s,17,Playback(pof-voipjet)
exten =&gt; s,18,Dial(IAX2/${ARG3}@${ARG4}/011${ARG1:3},60,g)
exten =&gt; s,19,NoOp(Contestat:${ANSWEREDTIME})
exten =&gt; s,20,GotoIf($["${ANSWEREDTIME}" = ""]?21:29)
exten =&gt; s,21,Dial(IAX2/${ARG3}@${ARG5}/011${ARG1:3},60,g)
exten =&gt; s,22,NoOp(Contestat:${ANSWEREDTIME})
exten =&gt; s,23,GotoIf($["${ANSWEREDTIME}" = ""]?24:29)
exten =&gt; s,24,Dial(IAX2/${ARG3}@${ARG6}/011${ARG1:3},60,g)
exten =&gt; s,25,NoOp(Contestat:${ANSWEREDTIME})
exten =&gt; s,26,GotoIf($["${ANSWEREDTIME}" = ""]?27:29)
exten =&gt; s,27,Playback(pof-voipbuster)
exten =&gt; s,28,Dial(IAX2/pauoliva@voipbuster-pau-out/00${ARG1:3},60) ; for voipbuster
exten =&gt; s,29,Hangup
</pre>
<p>Fins aquí les 3 macros que tinc definides, si queden dubtes als comentaris :)</p>
<p>Ara anem a veure la resta de contextos definits en el dialplan. Bàsicament l'he dividit en 6 parts:</p>
<ol>
<li>Context per defecte</li>
<li>Extensions d'usuaris</li>
<li>Trucades a l'exterior utilitzant prefixes per enrutar</li>
<li>Trucades a l'exterior sense marcar cap prefix previ</li>
<li>Trucades entrants</li>
<li>Contextos d'usuaris per separar privilegis</li>
</ol>
<h3>Context per defecte</h3>
<p>La part que veureu a continuació és el context per defecte, on van a parar les trucades entrants que és fan a la linea fixa de <a href="http://www.auna.es">Menta</a> connectada a la <a href="/blog/2005/04/21/x100p-oem-fxo-pci-card/">X100P</a> i les trucades que es fan al número de mòbil <a href="http://www.movistar.es">Movistar</a> que tinc connectat al trunk.</p>
<p>Primer defineixo les extensions "<code>s</code>" (start), "<code>t</code>" (timeout) i "<code>i</code>" (invalid).</p>
<pre>
;--------------------------------------------
; context per defecte (default), aqui entren totes les trucades
;--------------------------------------------

[default]

exten =&gt; s,1,Wait,1                             ; Wait 1 seconds
exten =&gt; t,1,Goto(#,1)                          ; If they take too long, give up
exten =&gt; i,1,Playback(invalid)                  ; "That's not valid, try again"
</pre>
<p>La extensió "<code>s</code>" s'executa quan entra una trucada, faig que s'espere 1 segón abans de contestar la trucada.</p>
<p>Redefineixo la extensió "<code>t</code>" per a que quan es produeixi un timeout vagi a executar les comandes de la extensió "<code>#</code>" a partir de la primera linia. Ho veurem amb més detall més endavant.</p>
<p>Quan l'usuari marca una extensió invalida, l'informo amb un missatge de veu.</p>
<pre>
; pof
exten =&gt; s/${POFMOV},2,Answer                   ; contesta
exten =&gt; s/${POFMOV},3,DigitTimeout(5)          ; set digit timeout 5 sec
exten =&gt; s/${POFMOV},4,ResponseTimeout(20)      ; response timeout 20 sec
exten =&gt; s/${POFMOV},5,Authenticate(xxxxx)      ; password
exten =&gt; s/${POFMOV},6,DISA(no-password|pofhq)  ; si el password es correcte transfereix al contexte

; esteve
exten =&gt; s/${ESTEVEMOV},2,Answer                        ; contesta
exten =&gt; s/${ESTEVEMOV},3,DigitTimeout(5)               ; set digit timeout 5 sec
exten =&gt; s/${ESTEVEMOV},4,ResponseTimeout(20)           ; response timeout 20 sec
exten =&gt; s/${ESTEVEMOV},5,Authenticate(xxxxx)           ; password
exten =&gt; s/${ESTEVEMOV},6,DISA(no-password|esteve)      ; si el password es correcte transfereix al contexte
</pre>
<p>Si la trucada ve del meu mòbil o del mòbil de l'Esteve, demano un password i si és correcte envio la trucada al contexte de l'usuari (meu o de l'Esteve), que permetrá realitzar trucades amb els nostres proveïdors.</p>
<p>Això el que em permet és per exemple, si he de trucar a USA i estic fora de casa marco el número del mòbil movistar del trunk desde el meu mòbil, la trucada fins al movistar em surt <a href="http://www.movistar.es/particulares/tarifasycontratos/ahorra/activa-mifavorito.htm">per 1 centim/minut</a> ja que el tinc com a "mi favorito", i llavors poso el password que he definit anteriorment i l'asterisk em dona tó al meu context, des d'on puc marcar el número de USA que s'enrutarà per VoipBuster amb la macro definida anteriorment (macpof-dialout) i trucaré gratis a USA :)</p>
<p>A continuació veiem la extensió "<code>#</code>" que he comentat anteriorment, on arribem quan es produeix un <em>timeout</em>:</p>
<pre>
exten =&gt; #,1,Answer                             ; contesta
exten =&gt; #,2,DigitTimeout(5)                    ; set digit timeout 5 sec
exten =&gt; #,3,ResponseTimeout(20)                ; response timeout 20 sec
exten =&gt; #,4,Background(pof-calabria)           ; Let them know what's going on
exten =&gt; #,5,DISA(no-password|usuaris)          ; transfereix al contexte sense demanar paswd
</pre>
<p>Aquí arriben les trucades que tenen un CallerID diferent dels que hem especificat anteriorment (tots els números, excepte el meu mòbil i el de l'Esteve). El que faig és reproduir un missatge de veu on informo a la persona que truca de que pot marcar el 200 per trucar-me a mi , el 201 per trucar a l'Esteve, etc...</p>
<p>Seguidament definim el mític <em>echo test</em>, per comprovar la qualitat de l'enllaç. Arribem al <em>echo test</em> marcant el 600.</p>
<pre>
exten =&gt; 600,1,Playback(demo-echotest)          ; Let them know what's going on
exten =&gt; 600,2,Echo                             ; Do the echo test
exten =&gt; 600,3,Playback(demo-echodone)          ; Let them know it's over
exten =&gt; 600,4,Hangup                           ; Start over
</pre>
<p>Finalment, definexio dues extensions la 8500 i la 8501 per a consultar la bustia de veu.</p>
<pre>
; Servei de contestador automàtic, per callerID
exten =&gt; 8500,1,VoiceMailMain(${CALLERIDNUM})
exten =&gt; 8500,2,Hangup

; Per accedir al contestador d'un altre usuari (demana extensio)
exten =&gt; 8501,1,VoicemailMain
exten =&gt; 8501,2,Hangup
</pre>
<p>La extensió 8500 ens portarà directament a la bustia de veu de l'usuari que fa la trucada, només ens demanarà el password. Si volem consultar la bustia de veu d'un altre usuari podem fer-ho marcant el 8501, llavors ens demanarà la <em>mailbox</em> i el password.</p>
<h3>Extensions dusuaris</h3>
<p>A continuació definim les extensions SIP dels usuaris del Asterisk:</p>
<pre>
;--------------------------------------------
; extensions d'usuaris
;--------------------------------------------

[sip]
;Pau
exten =&gt; 200,1,Dial(SIP/200,20,t)
exten =&gt; 200,2,Voicemail(200)
exten =&gt; 200,3,Hangup
exten =&gt; 200,102,Voicemail(200)
exten =&gt; 200,103,Hangup

;Esteve
exten =&gt; 201,1,Dial(SIP/201,20,t)
exten =&gt; 201,2,Voicemail(201)
exten =&gt; 201,3,Hangup
exten =&gt; 201,102,Voicemail(201)
exten =&gt; 201,103,Hangup

;Diego
exten =&gt; 203,1,Dial(SIP/203,20,t)
exten =&gt; 203,2,Voicemail(203)
exten =&gt; 203,3,Hangup
exten =&gt; 203,102,Voicemail(203)
exten =&gt; 203,103,Hangup
</pre>
<p>Tots els usuaris estàn creats de la mateixa manera, com veieu, si l'usuari no contesta la trucada en 20 segons s'envia al Voicemail.</p>
<h3>Trucades a lexterior utilitzant prefixes per enrutar</h3>
<p>L'objectiu d'aquesta part del dialplan és fer que puguem decidir per on s'enruta la trucada marcant un prefix abans del número al que volem trucar. Despres veurem com fer que la trucada s'enrute automàticament per on més ens interessi sense marcar prefix, però de vegades ens interessa poder desviar una trucada per una determinada ruta, i això és el que fem aquí.</p>
<p>Cada proveïdor té uns requisits respecte a com s'ha de marcar, si s'han de posar prefixes o no, etc... Amb aquest dialplan el que fem es unificar-los tots, de manera que truquem per on truquem no ens haugem de recordar dels prefixes que el proveïdor ens obliga a marcar, sino que l'Asterisk ho farà per nosaltres. Sempre utilitzarem la següent estructura per trucar:</p>
<p class="center"><code>prefix_de_ruta</code> + <code>codi_de_pais</code> + <code>num_de_telefon</code>.</p>
<p>Els prefixos de ruta he fet que comencin sempre per zero, ja que no hi ha cap país que tingui aquest prefix, així podem combinar els dos sistemes (trucades amb prefix de ruta i sense).</p>
<pre>
;--------------------------------------------
; trucades al exterior, extensions predefinides
;--------------------------------------------

[menta-out] ;055+num
exten =&gt; _05534[69]XXXXXXXX,1,Playback(pof-menta)
exten =&gt; _05534[69]XXXXXXXX,2,Dial(Zap/1/067${EXTEN:5},60)
exten =&gt; _05534[69]XXXXXXXX,3,Hangup
exten =&gt; _055[69]XXXXXXXX,1,Playback(pof-menta)
exten =&gt; _055[69]XXXXXXXX,2,Dial(Zap/1/067${EXTEN:3},60)
exten =&gt; _055[69]XXXXXXXX,3,Hangup
</pre>
<p>El codi que teniu a sobre d'aquestes linies pertany al prefixe predefinit per trucar amb la PSTN (linia de menta), per trucar marcarem 055 + numero de fixe o mobil on volem trucar. Podem posar el 34 davant o no posar-lo. Només deixo marcar a números que comencin per 6 o 9 (la majoría de mòbils i fixes, encara que també inclouria 900 o 901, 902..).</p>
<p>Fixeu-vos amb la sintàxis: el <acronym title="underline"><code>_</code></acronym> indica que el que ve a continuació és una expressió regular d'Asterisk, la sintaxis és la següent:</p>
<table class="listados">
<tbody>
<tr>
<td><strong>X</strong></td>
<td>Qualsevol digit del 0 al 9</td>
</tr>
<tr>
<td><strong>Z</strong></td>
<td>Qualsevol digit del 1 al 9</td>
</tr>
<tr>
<td><strong>N</strong></td>
<td>Qualsevol digit del 2 al 9</td>
</tr>
<tr>
<td><strong>[1237-9]</strong></td>
<td>Qualsevol digit entre els corxets, en aquest exemple 1,2,3,7,8,9.</td>
</tr>
<tr>
<td><strong>.</strong></td>
<td>El punt es un <em>wilcard</em>, indica qualsevol nombre de caràcters (un o més digits)</td>
</tr>
</tbody>
</table>
<p>A continuació us deixo la resta dels prefixes predefinits, que pertanyen als operadors que he comentat al prinicipi del post. Si heu entés l'exemple anterior i la sintàxis correctament no tindreu problema en entendre la resta de prefixes.</p>
<pre>
[fwdout-out] ;056+num
exten =&gt; _056.,1,SetCallerID,pau@eslack.org
exten =&gt; _056.,2,Playback(pof-fwdout)
exten =&gt; _056.,3,Dial(IAX2/${POFFWDOUT}@fwdOUT/${EXTEN:3},60)
exten =&gt; _056.,4,Hangup

[iaxfwd] ;057+num
; echo test: 057613, coffe house: 057614, trucar a VozTelecom: 057**868+num
exten =&gt; _057.,1,SetCallerID,${FWDCIDNAME}
exten =&gt; _057.,2,Playback(pof-fwdup)
exten =&gt; _057.,3,Dial(IAX2/${FWDNUMBER}@fwd-gw/${EXTEN:3},60)
exten =&gt; _057.,4,Hangup

[outgoing_voipuser-pof] ;058+num
exten =&gt; _058.,1,Playback(pof-voipuser)
exten =&gt; _058.,2,Dial(SIP/00${EXTEN:3}@voipuser-pof,60)
exten =&gt; _058.,3,Hangup

[outgoing_voipuser-esteve] ;058+num
exten =&gt; _058.,1,Playback(pof-voipuser)
exten =&gt; _058.,2,Dial(SIP/00${EXTEN:3}@voipuser-esteve,60)
exten =&gt; _058.,3,Hangup

[movistar-out] ;060+num
exten =&gt; _06034[69]XXXXXXXX,1,Playback(pof-movistar)
exten =&gt; _06034[69]XXXXXXXX,2,Dial(Zap/2/#31#${EXTEN:5},60)
exten =&gt; _06034[69]XXXXXXXX,3,Hangup
exten =&gt; _060.,1,Playback(pof-movistar)
exten =&gt; _060.,2,Dial(Zap/2/#31#${EXTEN:3},60)
exten =&gt; _060.,3,Hangup

[adamvoip] ;061+num
; 0611114 --&gt; conferencia / 0611111 --&gt; echo test
exten =&gt; _061.,1,Playback(pof-adam)
exten =&gt; _061.,2,Dial(SIP/${EXTEN:3}@adamvoip,60)
exten =&gt; _061.,3,Hangup

[voipjet-pau-out] ;011+num
exten =&gt; _0111NXXNXXXXXX,1,Macro(macvoipjet,${EXTEN},${POFMOV},${POFVOIPJET},voipjet-pau-out,voipjet-pau-out2,voipjet-pau-out3)
exten =&gt; _0111NXXNXXXXXX,2,Hangup
exten =&gt; _011.,1,Macro(macvoipjet,${EXTEN},${POFMOV},${POFVOIPJET},voipjet-pau-out,voipjet-pau-out2,voipjet-pau-out3)
exten =&gt; _011.,2,Hangup

[voipjet-esteve-out] ;011+num
exten =&gt; _0111NXXNXXXXXX,1,Macro(macvoipjet,${EXTEN},${ESTEVEMOV},${ESTEVEVOIPJET},voipjet-esteve-out,voipjet-esteve-out2,voipjet-esteve-out3)
exten =&gt; _0111NXXNXXXXXX,2,Hangup
exten =&gt; _011.,1,Macro(macvoipjet,${EXTEN},${ESTEVEMOV},${ESTEVEVOIPJET},voipjet-esteve-out,voipjet-esteve-out2,voipjet-esteve-out3)
exten =&gt; _011.,2,Hangup

[voipjet-diego-out] ;011+num
exten =&gt; _0111NXXNXXXXXX,1,Macro(macvoipjet,${EXTEN},${DIEGOMOV},${DIEGOVOIPJET},voipjet-diego-out,voipjet-diego-out2,voipjet-diego-out3)
exten =&gt; _0111NXXNXXXXXX,2,Hangup
exten =&gt; _011.,1,Macro(macvoipjet,${EXTEN},${DIEGOMOV},${DIEGOVOIPJET},voipjet-diego-out,voipjet-diego-out2,voipjet-diego-out3)
exten =&gt; _011.,2,Hangup

[voipbuster-pau-out]
exten =&gt; _012.,1,SetCallerID,${POFMOV}
exten =&gt; _012.,2,Playback(pof-voipbuster)
exten =&gt; _012.,3,Dial(IAX2/pauoliva@voipbuster-pau-out/00${EXTEN:3},60)
exten =&gt; _012.,4,Hangup
</pre>
<p>Fixeu-vos amb l'ús de la macro <tt>macvoipjet</tt> per a les trucades amb VoipJet. També hi ha un parell de detalls, com per exemple l'ús de <code>067</code> davant dels números que surten pel fixe i de <code>#31#</code> davant dels números que surten pel mòbil, això el que fa és amagar el CallerID.</p>
<p>Com sempre, si teniu algun dubte o suggerència utilitzeu els comentaris.</p>
<h3>Trucades a lexterior sense marcar cap prefix previ</h3>
<p>Normalment el que ens interessarà serà marcar el numero de telèfon al que volem trucar i deixar que l'Asterisk "decideixi" per quina ruta enviar la trucada automàticament, sense haver-nos de preocupar de marcar un prefixe com hem fet en l'apartat anterior. A continuació veureu la part del dialplan que s'encarrega de ---en funció del codi de país i de la longitud del número marcat--- sel·leccionar el proveïdor que ofereixi la tarifa més econòmica per a dur a terme la trucada. Si per qualsevol raó aquest proveïdor no estigués disponible intentarem sempre fer la trucada amb el següent proveïdor que ens oferís la millor tarifa.</p>
<p>Comencem enrutant els números gratuïts (<em>toll-free</em>) dels paísos on podem arribar sense pagar:</p>
<pre>
;--------------------------------------------
; trucades al exterior
;--------------------------------------------

[tollfree]
; els paisos que conec quina longitud té la numeració estan exactes
; els que no ho conec tenen un . q pot ser qualsevol longitud

;=========USA=======
exten =&gt; _1800NXXXXXX,1,Macro(mactollfree,${EXTEN})
exten =&gt; _1700NXXXXXX,1,Macro(mactollfree,${EXTEN})
exten =&gt; _1888NXXXXXX,1,Macro(mactollfree,${EXTEN})
exten =&gt; _1877NXXXXXX,1,Macro(mactollfree,${EXTEN})
exten =&gt; _1866NXXXXXX,1,Macro(mactollfree,${EXTEN})

;=========Netherlands=======
exten =&gt; _31800.,1,Macro(mactollfree,${EXTEN})

;=========UK=======
exten =&gt; _44800XXXXXXX,1,Macro(mactollfree,${EXTEN})
exten =&gt; _44500XXXXXXX,1,Macro(mactollfree,${EXTEN})
exten =&gt; _44808XXXXXXX,1,Macro(mactollfree,${EXTEN})

;=========Norway=======
exten =&gt; _47800.,1,Macro(mactollfree,${EXTEN})

;=========Germany=======
exten =&gt; _49800.,1,Macro(mactollfree,${EXTEN})
exten =&gt; _49130.,1,Macro(mactollfree,${EXTEN})

;=========Spain========
exten =&gt; _900XXXXXX,1,Playback(pof-menta)
exten =&gt; _900XXXXXX,2,Dial(Zap/1/067${EXTEN},60)
exten =&gt; _900XXXXXX,3,Hangup
exten =&gt; _34900XXXXXX,1,Playback(pof-menta)
exten =&gt; _34900XXXXXX,2,Dial(Zap/1/067${EXTEN:2},60)
exten =&gt; _34900XXXXXX,3,Hangup
</pre>
<p>Fixeu-vos que si conec la longitud del número la especifíco amb "<code>X</code>", i si no poso un "<code>.</code>" que indica qualsevol longitud. Per als <em>toll-free</em> internacionals utilitzo la macro <tt>mactollfree</tt> que hem comentat al inici del post, per al 900 (espanya) utilitzo la línea de menta.</p>
<p>A continuació definirem que fer amb les trucades a fixes d'espanya, deixem trucar tant amb un <code>34</code> al davant com sense, com si la trucada es fes des d'una línea espanyola. La trucada s'enrutarà per defecte per la línea fixa de menta que té trucades nacionals gratuites, en cas de que la linea estigués ocupada faríem sortir la trucada per VoipBuster, que també ofereix trucades gratuïtes als fixes d'Espanya.</p>
<pre>
[spain-free]
; trucades a fixes d'espanya, primer provem per menta
; si no va entrutem per voipbuster que també son gratis
exten =&gt; _9ZXXXXXXX,1,Playback(pof-menta)
exten =&gt; _9ZXXXXXXX,2,Dial(Zap/1/067${EXTEN},60,g)
exten =&gt; _9ZXXXXXXX,3,NoOp(Contestat:${ANSWEREDTIME})
exten =&gt; _9ZXXXXXXX,4,GotoIf($["${ANSWEREDTIME}" = ""]?5:6)
exten =&gt; _9ZXXXXXXX,5,Dial(IAX2/pauoliva@voipbuster-pau-out/0034${EXTEN},60)
exten =&gt; _9ZXXXXXXX,6,Hangup

exten =&gt; _349ZXXXXXXX,1,Playback(pof-menta)
exten =&gt; _349ZXXXXXXX,2,Dial(Zap/1/067${EXTEN:2},60,g)
exten =&gt; _349ZXXXXXXX,3,NoOp(Contestat:${ANSWEREDTIME})
exten =&gt; _349ZXXXXXXX,4,GotoIf($["${ANSWEREDTIME}" = ""]?5:6)
exten =&gt; _349ZXXXXXXX,5,Dial(IAX2/pauoliva@voipbuster-pau-out/00${EXTEN},60)
exten =&gt; _349ZXXXXXXX,6,Hangup
</pre>
<p>Fixeu-vos en que aquí he utilitzat la "<code>Z</code>", ja que els fixes sempre porten un número del 1 al 9 després del 9. Si hagués posat una <code>X</code> en lloc d'una <code>Z</code> se m'haguessen pogut colar les trucades a números especials com els 901 o 902.</p>
<p>Ara veurem que fer amb les trucades a mòbils. El trunk que tinc connectat té un SIM Movistar de prepagament amb la tarifa <a href="http://www.movistar.es/particulares/tarifasycontratos/modalidades/activatutiempo.htm">activa tu tiempo</a>, que surt per 12 centims/minut de les 16h fins a les 4 del matí, i a 48 centims/minut la resta d'hores. Les hores que la tarifa de movistar surt a 12 centims enrutaré les trucades pel trunk, i la resta d'hores les enrutaré a través de VoipJet que les trucades a mòbils d'Espanya surten a 19,6 centims / minut. I finalment, si VoipJet estigués caigut fariem sortir la trucada per VoipBuster que tindría un cost de 22,6 centims/minut.</p>
<p>Per això que us comento, utilitzo la comanda <tt>GotoIfTime</tt> de la següent manera:</p>
<pre>
[callout-pau]
; les trucades a mòbil van pel trunk (només de 16 a 4h)
exten =&gt; _346XXXXXXXX,1,GotoIfTime(03:55:01-16:05:00,mon-fri,*,*?callout-normal,${EXTEN},1)
exten =&gt; _346XXXXXXXX,2,Playback(pof-movistar)
exten =&gt; _346XXXXXXXX,3,Dial(Zap/2/#31#${EXTEN:2},60)
exten =&gt; _346XXXXXXXX,4,Hangup

exten =&gt; _6XXXXXXXX,1,GotoIfTime(03:55:01-16:05:00,mon-fri,*,*?callout-normal,${EXTEN},1)
exten =&gt; _6XXXXXXXX,2,Playback(pof-movistar)
exten =&gt; _6XXXXXXXX,3,Dial(Zap/2/#31#${EXTEN},60)
exten =&gt; _6XXXXXXXX,4,Hangup

<span class="red">exten =&gt; _XXXXXXXXXX.,1,Macro(macpof-dialout,011${EXTEN},${POFMOV},${POFVOIPJET},voipjet-pau-out,voipjet-pau-out2,voipjet-pau-out3)
exten =&gt; _XXXXXXXXXX.,2,Hangup</span>


[callout-normal]
; les trucades a mòbil de les 4 de la nit a les 16h van per voipjet
exten =&gt; _346XXXXXXXX,1,Macro(macpof-dialout,011${EXTEN},${POFMOV},${POFVOIPJET},voipjet-pau-out,voipjet-pau-out2,voipjet-pau-out3)
exten =&gt; _6XXXXXXXX,1,Macro(macpof-dialout,01134${EXTEN},${POFMOV},${POFVOIPJET},voipjet-pau-out,voipjet-pau-out2,voipjet-pau-out3)
</pre>
<p>Amb els paràmetres de la comanda <tt>GotoIfTime</tt> indico que si l'hora està compresa entre les <code>03:55:01</code> i les <code>16:05:00</code> de dilluns a divendres (<code>mon-fri</code>... els caps de setmana la activa tutiempo em deixa trucar a 12 centims/min tot el dia) envïi la trucada a la linea <code>1</code> del contexte <code>[callout-normal]</code>, definit a sota de <code>[callout-pau]</code>. En cas contrari seguiría a la linea 2 de <code>[callout-pau]</code>, que s'encarregará de fer sortir la trucada pel trunk amb Movistar.</p>
<p>Un altre punt a tenir en compte és el final del context <code>[callout-pau]</code> ---marcat en <span class="red">vermell</span>---, que sería com una mena de <em>ruta per defecte</em> o <em>catch-all</em>, el que indica és: "tots els números que tinguin més de 10 digits envials cap a la macro <tt>macpof-dialout</tt>", que si recordeu el que fa es provar els 3 servers de voipjet, i si no funciona VoipJet enrutar la trucada per VoipBuster. És molt important que aquesta línea estigui en aquesta posició del dialplan, ja que aquí ja hem enrutat les trucades a numeros gratuïts, fixes i mòbils d'espanya per on voliem. La limitació de "més de 10 digits" és per que tots els números internacionals tenen com a mínim 11 digits (si incloem el codi del pais).</p>
<p>NOTA: Si has segut capaç de seguir tot el que anat explicant fins ara (m'enrotllo com una persiana!) i estàs al dia de les tarifes de VoipJet i VoipBuster te'n hauràs adonat de que aquest dialplan no és perfecte... si no te'n has adonat, passo a explicar el "problema" que té, i com posar-hi remei de forma cutre...</p>
<p>El problema aquí és que per defecte totes les trucades a números internacionals (excepte USA) surten per VoipJet, que és l'operador més econòmic, <strong>però</strong> amb VoipBuster tenim trucades gratuites a fixes de Australia, Austria, Belgica, República Checa, Dinamarca, Finalndia, França, Alemania, Grecia, Irlanda, Luxemburg, Holanda, Noruega, Portugal, Suècia, Suissa i Anglaterra... Com no em sé tots els prefixes de fixes i mòbils de tots aquests països no he pogut implementar que fassi la trucada cap als fixes a través de VoipBuster en lloc de a través de VoipJet, per tant si truquem a un fixe de per exemple Holanda, la trucada s'enrutaría per VoipJet i pagariem. Per evitar aquest problema hem de tenir en compte que si truquem a un telèfon fixe d'un d'aquests països hem de marcar el <code>012</code> al davant, obligant així a que la trucada s'enrute a través de VoipBuster (amb els prefixes estàtics predefinits que he explicat al apartat anterior) i sigui gratuita. Si algú sap alguna manera d'automatitzar això, o coneix on trobar un llistat actualitzat de tots els prefixes de fixos i mòbils d'aquests països estaría molt agraït de que ho compartís amb tothom :)</p>
<p>A continuació la resta del dialplan per a l'Esteve i el Diego, que no tenen account de VoipBuster ni accés al Movistar del trunk, per tant tot surt per VoipJet.</p>
<pre>
[callout-esteve]
; les trucades van directes per voipjet:
exten =&gt; _346XXXXXXXX,1,Macro(macvoipjet,011${EXTEN},${ESTEVEMOV},${ESTEVEVOIPJET},voipjet-esteve-out,voipjet-esteve-out2,voipjet-esteve-out3)
exten =&gt; _346XXXXXXXX,2,Hangup
exten =&gt; _6XXXXXXXX,1,Macro(macvoipjet,01134${EXTEN},${ESTEVEMOV},${ESTEVEVOIPJET},voipjet-esteve-out,voipjet-esteve-out2,voipjet-esteve-out3)
exten =&gt; _6XXXXXXXX,2,Hangup
exten =&gt; _XXXXXXXXXX.,1,Macro(macvoipjet,011${EXTEN},${ESTEVEMOV},${ESTEVEVOIPJET},voipjet-esteve-out,voipjet-esteve-out2,voipjet-esteve-out3)
exten =&gt; _XXXXXXXXXX.,2,Hangup


[callout-diego]
exten =&gt; _346XXXXXXXX,1,Macro(macvoipjet,011${EXTEN},${DIEGOMOV},${DIEGOVOIPJET},voipjet-diego-out,voipjet-diego-out2,voipjet-diego-out3)
exten =&gt; _346XXXXXXXX,2,Hangup
exten =&gt; _6XXXXXXXX,1,Macro(macvoipjet,011${EXTEN},${DIEGOMOV},${DIEGOVOIPJET},voipjet-diego-out,voipjet-diego-out2,voipjet-diego-out3)
exten =&gt; _6XXXXXXXX,2,Hangup
exten =&gt; _XXXXXXXXXX.,1,Macro(macvoipjet,011${EXTEN},${DIEGOMOV},${DIEGOVOIPJET},voipjet-diego-out,voipjet-diego-out2,voipjet-diego-out3)
exten =&gt; _XXXXXXXXXX.,2,Hangup
</pre>
<p>Fixeu-vos en el cas anterior, com "arreglo" el número que enviem a la macro <tt>macvoipjet</tt> posant el 34 al davant en el cas de que marquen el número de mòbil espanyol sense posar el codi del país. Fixeu-vos també que en aquest cas la primera linea de cada contexte sería redundant, ja que la última contempla el mateix cas.</p>
<h3>Trucades entrants</h3>
<p>En aquesta secció veurem com tractar les trucades entrants que no arriben per cap interficie Zap, ja que aquestes les hem tractat al context <code>[default]</code>.</p>
<p>En resum, tenim trucades que poden arribar al meu número de <acronym title="Free World DialUP">FWD</acronym>, trucades que poden arribar a través de <acronym title="Free World DialOUT">FWDOUT</acronym> i trucades que poden arribar al meu <a href="/blog/2005/06/21/voipuser-uk-did-trucades-a-pstn-gratuites/">DID de Voipuser</a>.</p>
<p>Aquí us deixo la configuració bàsica, aviat faré un altre post (espero que no tan llarg com aquest!) amb algun truquillo que podem fer servir aquí... ;)</p>
<pre>
;--------------------------------------------
; trucades entrants
;--------------------------------------------

[fromiaxfwd]
exten =&gt; ${FWDNUMBER},1,Dial(${FWDRINGS},20)
exten =&gt; ${FWDNUMBER},2,Voicemail,u${FWDVMBOX}
exten =&gt; ${FWDNUMBER},102,Voicemail,b${FWDVMBOX}
</pre>
<p>Quan arriba una trucada cap al número de <acronym title="Free World DialUP">FWD</acronym> definit a la variable ${FWDNUMBER} truquem a la extensió definida a la variable ${FWDRINGS} (la 200, la meva extensió). Si contesto la trucada el procés finalitza aquí (no s'executen la resta de comandes (<code>2</code> i <code>102</code>). Si en <code>20</code> segons no contesto passem a la seqüència <code>2</code>, on la trucada serà atesa pel meu Voicemail com a <em>unanswered</em> (això ho indica la <code>u</code> que passem davant de la extensió ${FWDVMBOX}. En el cas de que jo estigués comunicant, la comanda <tt>Dial</tt> salta a la sequència actual + 101, per tant aniría a parar al <code>102</code>, on la trucada serà atesa pel Voicemail com a <em>busy</em> (ho indica la <code>b</code>).</p>
<p>El cas de <acronym title="Free World DialOUT">FWDOUT</acronym> és una mica més complicat, veiem-lo a continuació:</p>
<pre>
[fromfwdOUT] ;match the context in iax.conf
; tots els fixes (349...) excepte els 90X, ej 908 902...
exten =&gt; _349ZXXXXXXX,1,Dial(ZAP/1/067${EXTEN:2},60,T,L(1800000:1790000))
; tots els 900, son num. gratuits...
exten =&gt; _34900XXXXXX,1,Dial(ZAP/1/067${EXTEN:2},60,T,L(1800000:1790000))
include =&gt; fromfwdOUT-catchall

[fromfwdOUT-catchall]
exten =&gt; _X.,1,NoOp(--- ${CALLERID} calling to (${EXTEN}) ---)
exten =&gt; _X.,2,Hangup
exten =&gt; h,1,Hangup     ;hangup event
exten =&gt; i,1,Hangup     ;invalid event
exten =&gt; t,1,Hangup     ;timeout event
</pre>
<p>Aquí recollim totes les trucades que ens arriben per <acronym title="Inter Asterisk eXchange">IAX</acronym> amb la <a href="/blog/2005/06/13/free-world-dialout-com-trucar-gratis-a-qualsevol-lloc-del-mon/">ruta que publico a <acronym title="Free World DialOUT">FWDOUT</acronym></a>. Aquestes trucades entren al context <code>[fromfwdOUT]</code> on es chequeja el número al que vol trucar la persona que fa la trucada, si el número coincideix amb la expressió <code>_349ZXXXXXXX</code> (34 (espanya) + 9 + un número del 1 al 9 + 7 digits més)  o <code>_34900XXXXXX</code> (34 (espanya) + 900 + 6 digits més), es a dir, és un número fixe o un 900 que em surt gràtis amb Menta, el deixo sortir per la linea de Menta (<code>Zap/1</code>), li afegeixo un <code>067</code> al davan per a que oculte el CallerID. Amb la <code>T</code> permetem que l'usuari que truca pugui fer <em>transfer</em> de la trucada, i amb <code>L(1800000:1790000)</code> limitem la trucada a 1800000 ms (30 minuts), avisant a l'usuari amb un missatge que diu "<em>you have 30 minutes for this call</em>" quan falten 1790000 ms (30 minuts menys 10 segons, es a dir, al principi de la trucada) per a que li tallem la trucada.</p>
<p>Si l'usuari intentés trucar a un número que no coincidís amb la expressió que hem definit al principi, li fem un <tt>Hangup</tt>, això ho aconseguim incloent el context <code>[fromfwdOUT-catchall]</code> al final del context <code>[fromfwdOUT]</code>. Aquest context <em>logueja</em> amb un <tt>NoOp</tt> l'intent de trucada i la finalitza.</p>
<p>Ara veurem el cas de Voipuser, aquí tinc dos contextos molt semblants, un  per l'Esteve i l'altre per a mi, ja que els dos tenim un compte de Voipuser.</p>
<pre>
[incoming_voipuser-pof]
exten =&gt; 448449862541,1,NoOp(--- ${CALLERID} calling on pof-VoIPUser (${EXTEN}) ---)
exten =&gt; 448449862541,2,Dial(SIP/200,20)
exten =&gt; 448449862541,3,Answer
exten =&gt; 448449862541,4,Wait,1
exten =&gt; 448449862541,5,Voicemail(u200)
exten =&gt; 448449862541,6,HangUp

[incoming_voipuser-esteve]
exten =&gt; 448449862188,1,NoOp(--- ${CALLERID} calling on esteve-VoIPUser (${EXTEN}) ---)
exten =&gt; 448449862188,2,Dial(SIP/201,20)
exten =&gt; 448449862188,3,Answer
exten =&gt; 448449862188,4,Wait,1
exten =&gt; 448449862188,5,Voicemail(u201)
exten =&gt; 448449862188,6,HangUp
</pre>
<p>El que fem és <em>loguejar</em> la trucada amb l'ajuda de la comanda <tt>NoOp</tt>, trucar al usuari (200 en el meu cas, 201 en el del esteve). Si l'usuari contesta el <tt>Dial</tt> finalitzarà el flux d'execució (no s'executaran més comandes del context), en cas contrari l'Asterisk contestarà al cap de 20 segons, esperarà 1 segon i enviarà la trucada al Voicemail del usuari amb el missatge de <em>unanswered</em>.</p>
<h3>Contextos dusuaris per separar privilegis</h3>
<p>Finalment, anem a veure <em>qui pot fer què?</em>... això ho definim creant un context específic per a cada usuari i incloent-hi els contextos ---que hem definit prèviament--- als que li volem donar accés.</p>
<pre>
;--------------------------------------------
; contextos per separar privilegis
;--------------------------------------------
[usuaris] include =\> default include =\> sip

Primer creem un context `[usuaris]`, on només incloem el context `[default]` on entren les trucades, i les extensions que hem definit per a cada usuari (context `[sip]`). D'aquesta manera una trucada que entra per la <acronym title="Public Switched Telephone Network">PSTN</acronym> de Menta pot arribar a la extensió d'un usuari, (jo al 200, l'esteve al 201, el diego al 203...) i li podem contestar o ens pot deixar un missatge al voicemail si no tenim cap telèfon <acronym title="Session Initiation Protocol">SIP</acronym> connectat.

```
[pofhq] include =\> usuaris include =\> iaxfwd include =\> spain-free include =\> tollfree include =\> menta-out include =\> movistar-out include =\> adamvoip include =\> fwdout-out include =\> outgoing\_voipuser-pof include =\> voipjet-pau-out include =\> voipbuster-pau-out include =\> callout-pau include =\> fromiaxfwd include =\> incoming\_voipuser-pof
```

El context `[pofhq]` que teniu a sobre d'aquestes línies és el que utilitzo jo, i per tant és el que té més privilegis i accés a la resta de contextos que hem definit. Fixeu-vos que és molt important l'ordre en que afegim els contextos, per a que no s'enrute una trucada per un lloc que no toca.

Bàsicament tinc accés al context d'usuaris (per poder trucar al telèfon SIP de l'Esteve per exemple), puc trucar amb <acronym title="Free World DialUP">FWD</acronym> (prefix 057), puc arribar al context `[spain-free]`, que enruta les trucades a fixes d'espanya primer per menta i si estigués ocupat per VoipUser, puc usar els números _toll-free_ (amb <acronym title="Free World DialUP">FWD</acronym> i <acronym title="Free World DialOUT">FWDOUT</acronym> i menta per als 900), puc sortir per la línia de Menta (prefix 055) per trucar a fixes i a mòbils, puc trucar a números de Adam (prefix 061), puc trucar amb <acronym title="Free World DialOUT">FWDOUT</acronym> (prefix 056) i Voipuser (prefix 058) a números fixes de qualsevol país, puc trucar per VoipJet (prefix 011), puc trucar amb VoipBuster (prefix 012) i puc fer que les trucades s'enruten _automàgicament_ sense marcar prefix (amb el context `[callout-pau]`). Finalment, també puc rebre trucades als meu número de <acronym title="Free World DialUp">FWD</acronym> i de Voipuser.

A continuació teniu els contextos de l'Esteve i el Diego, que si heu entès el meu no us costaran molt de desxifrar...

```
[esteve] include =\> usuaris include =\> iaxfwd include =\> spain-free include =\> tollfree include =\> menta-out include =\> outgoing\_voipuser-esteve include =\> voipjet-esteve-out include =\> callout-esteve include =\> incoming\_voipuser-esteve [diego] include =\> usuaris include =\> iaxfwd include =\> spain-free include =\> tollfree include =\> menta-out include =\> outgoing\_voipuser-esteve include =\> voipjet-diego-out include =\> callout-diego
```

I això és tot el dialplan que tinc configurat al Asterisk de casa, hi ha algunes cosetes més que me les guardo per propers posts perquè encara les estic implementant. Com sempre, espero que això pugui ser útil per algú, i les correccions de possibles errors o suggeriments son benvingudes en els comentaris :D.

PD: Talibans ortogràfics, perdoneu-me però no soc Cervantes i ara mateix no tinc cap corrector ortogràfic a mà! :P

