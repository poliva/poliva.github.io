---
layout: post
title: Disable Apache2 weak and medium ciphers for PCI compliance
date: 2011-06-07 10:51:41.000000000 +02:00
type: post
categories:
- linux
- security
tags:
- apache
- ASV
- cipher
- nessus
- PCI
- scan
- ssl
- sslv2
- tls
- vulnerability
meta:
  yourls_tweeted: '1'
permalink: "/2011/06/07/disable-apache2-weak-medium-ciphers-pci-compliance/"
---
A few days ago we had an external vulnerability scan by an Approved Scanning Vendor (ASV) to pass PCI DSS, in the report we saw these two vulnerabilities also reported by our [Nessus](www.nessus.org) scan:

**The remote service supports the use of medium strength SSL ciphers:**

<tt>The remote host supports the use of SSL ciphers that offer medium strength encryption, which we currently regard as those with key lengths at least 56 bits and less than 112 bits.</tt>

Nessus ID: [42873](http://www.nessus.org/plugins/index.php?view=single&id=42873)

**The remote service supports the use of weak SSL ciphers:**

<tt>The remote host supports the use of SSL ciphers that offer either weak encryption or no encryption at all.</tt>

Nessus ID: [26928](http://www.nessus.org/plugins/index.php?view=single&id=26928)

To fix this, you just have to change the apache2 ssl configuration as follows:

```
SSLProtocol -all +SSLv3 +TLSv1 SSLCipherSuite ALL:!aNULL:!ADH:!eNULL:!LOW:!EXP:RC4+RSA:+HIGH:+MEDIUM
```

After this change, restart the Apache webserver and run Nessus again. The warnings on those two vulnerabilities should now disappear.

