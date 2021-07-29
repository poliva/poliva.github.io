---
layout: post
title: From APK to readable java source code in 3 easy steps
date: 2011-02-18 13:21:05.000000000 +01:00
type: post
categories:
- android
- linux
- security
tags:
- android
- apk
- class
- decompile
- dex
- dex2jar
- jar
- java
- jd-gui
- jdgui
meta:
  yourls_tweeted: '1'
  _wp_old_slug: ''
permalink: "/2011/02/18/from-apk-to-readable-java-source-code-in-3-easy-steps/"
---
Android applications are packed inside a APK file, which is just a ZIP file containing among other things a compact [Dalvik Executable (.dex)](http://en.wikipedia.org/wiki/Dalvik_(software)) file.  
First step is to extract the "classes.dex" file from the APK:

```
$ unzip program.apk classes.dex Archive: program.apk inflating: classes.dex
```

Now, we use the tool [dex2jar](http://code.google.com/p/dex2jar/) to convert the classes.dex file to Java .class files:

```
$ bash dex2jar/dex2jar.sh ./classes.dex version:0.0.7.8-SNAPSHOT 2 [main] INFO pxb.android.dex2jar.v3.Main - dex2jar ./classes.dex -\> ./classes.dex.dex2jar.jar Done.
```

From here we obtain the file "classes.dex.dex2jar.jar", now we can use the java decompiler [JD-GUI](http://java.decompiler.free.fr/?q=jdgui) to extract the source code:

```
$ ./jd-gui classes.dex.dex2jar.jar
```

Now just go to "_File -\> Save all sources_" and it will generate the zip file "classes.dex.dex2jar.src.zip" containing all the decompiled Java source code :)

