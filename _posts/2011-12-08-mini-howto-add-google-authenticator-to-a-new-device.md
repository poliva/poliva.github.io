---
layout: post
title: 'mini-howto: add google authenticator to a new device'
date: 2011-12-08 18:43:44.000000000 +01:00
type: post
categories:
- android
- gadgets
- linux
- minipost
- security
tags:
- 2-step verification
- authenticator
- google authenticator
- new phone
meta:
  yourls_tweeted: '1'
permalink: "/2011/12/08/mini-howto-add-google-authenticator-to-a-new-device/"
---
Note: For this to work you need to have google authenticator working on a rooted device.

Google authenticator stores user data in a sqlite database, so we can just get the key from there and move it on a different device, the process is as follows:

```
$ adb pull /data/data/com.google.android.apps.authenticator/databases/databases $ sqlite3 ./databases sqlite\> select \* from accounts; 1|whatever@gmail.com|key|0|0 sqlite\> .exit
```

The third column contains the key you need to manually copy to the new phone. After that, google authenticator will work on both devices (you can check the time-based generated key is the same on both).

