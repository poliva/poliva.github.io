---
layout: post
title: Edit image resources inside an android APK file
date: 2011-02-18 12:52:50.000000000 +01:00
type: post
categories:
- android
- linux
tags:
- android
- apk
- resources
- sign
meta:
  yourls_tweeted: '1'
  _wp_old_slug: ''
permalink: "/2011/02/18/edit-image-resources-inside-an-android-apk-file/"
---
Today I had to change some image resources in a APK file, the process is easy once you know it, so I just post it here for future reference:

First use the tool "aapt" from the android SDK to list the resources:  
`$ANDROID/tools/aapt list file.apk`

Once we locate the resources that we need to change, we use "remove" and "add" to replace them:  
`
	$ANDROID/tools/aapt remove file.apk res/drawable/file.png
	$ANDROID/tools/aapt add file.apk res/drawable/file.png
`

Then we have to remove the old APK signature and replace it with a new one.  
We will generate a fake self signed key to sign the APK:  
`
$ openssl genrsa -out key.pem 1024
$ openssl req -new -key key.pem -out request.pem
$ openssl x509 -req -days 9999 -in request.pem -signkey key.pem -out certificate.pem
$ openssl pkcs8 -topk8 -outform DER -in key.pem -inform PEM -out key.pk8 -nocrypt
`

Remove the old signature from the APK:  
`
for f in `$ANDROID/tools/aapt list file.apk |grep "META-INF"` ; do
	$ANDROID/tools/aapt remove file.apk $f
done
`

And now we sign the APK, I use [signapk.jar](/archives/files/signapk.jar) to do this:  
`$ java -jar signapk.jar certificate.pem key.pk8 file.apk file-signed.apk`

That's it, our APK is now ready to install... just remember to setup your android phone to allow installing applications from unknown sources :)

