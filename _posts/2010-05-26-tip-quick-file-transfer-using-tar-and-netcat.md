---
layout: post
title: 'tip: quick file transfer using tar and netcat'
date: 2010-05-26 12:40:44.000000000 +02:00
type: post
categories: []
tags:
- tar netcat
meta:
permalink: "/2010/05/26/tip-quick-file-transfer-using-tar-and-netcat/"
---
sender:

```
tar c files | nc -w 10 -l -p 12345
```

receiver:

```
nc -w 10 sender\_ip 12345 | tar xvfp -
```
