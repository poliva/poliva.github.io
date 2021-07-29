---
layout: post
title: GeSHi syntax-colorer per posar-li color al codi
date: 2005-11-28 15:21:43.000000000 +01:00
type: post
categories:
- linux
- pofHQ
tags: []
meta:
  tags: geshi syntax code hylight php wordpress plugin
permalink: "/2005/11/28/geshi-syntax-colorer-per-posar-li-color-al-codi/"
---
Gràcies a [aquest post de Juanjo](http://blackshell.usebox.net/archivo/728.php) he descobert [GeSHi](http://qbnz.com/highlighter/), una clase de php que permet colorejar la sintaxis del codi de forma automàtica i suporta uns quants llenguàtges diferents. Per a integrar-lo amb [WordPress](http://www.wordpress.org) he usat [aquest plugin](http://dev.wp-plugins.org/browser/geshi-syntax-colorer/), aquí podeu veure els resultats:

Fragment de codi en PHP:

`
function get_mime_type(&$structure) {`

$primary\_mime\_type = array("TEXT", "MULTIPART", "MESSAGE", "APPLICATION", "AUDIO", "IMAGE", "VIDEO", "OTHER");  
 if($structure-\>subtype) {  
 return $primary\_mime\_type[(int) $structure-\>type] . '/' . $structure-\>subtype;  
 }  
 return "TEXT/PLAIN";  
}

Fragment de codi en bash script:

`
#!/bin/sh`

source ./include.sh

function error {  
 echo $1  
 exit 1  
}

function isfield() {

out=""  
 cfound="no"  
 lon=`echo -n $1 |wc -c`  
 lon=`echo $lon` # remove spaces  
 for ((i=1;i\< =$lon;i++)); do  
 cchar=`echo $1 |cut -c $i`  
 if ["$cchar" == "\""]; then  
 if ["$cfound" == "no"]; then cfound="yes" ; else cfound="no" ; fi  
 elif ["$cchar" == ","]; then  
 if ["$cfound" == "yes"]; then out="${out}\," ; else out="${out};" ; fi  
 else  
 out="${out}${cchar}"  
 fi  
 done  
 echo $out

}

A que queda bé? ;)

