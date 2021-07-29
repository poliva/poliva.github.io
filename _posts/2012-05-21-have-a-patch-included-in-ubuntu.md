---
layout: post
title: How to have your patch included in Ubuntu
date: 2012-05-21 22:56:05.000000000 +02:00
type: post
categories:
- linux
- minipost
tags:
- deb
- debian
- fix
- package
- packaging
- patch
- ubuntu
- wpasupplicant
meta:
  yourls_tweeted: '1'
permalink: "/2012/05/21/have-a-patch-included-in-ubuntu/"
---
Today I submitted a patch to wpa-supplicant package in Ubuntu ([LP: #994739](https://bugs.launchpad.net/bugs/994739)) and I learnt the process of pushing your changes to Launchpad and ask for them to be reviewed and merged. Ubuntu has a great documentation on how to fix bugs [in this Wiki](https://wiki.ubuntu.com/Bugs/HowToFix) and [packaging guide](http://developer.ubuntu.com/packaging/html/fixing-a-bug.html), just for future reference here are the steps I followed.

If you don't have them, get the ubuntu-dev-tools, and bazar VCS:

```
$ sudo apt-get install bzr ubuntu-dev-tools $ bzr whoami "Your Name \<you@example.org\>" $ bzr launchpad-login poliva
```

Then get the source of the package you want to fix, and apply your patch:

```
$ bzr branch lp:ubuntu/precise/wpasupplicant $ cd wpasupplicant $ patch -p1 \< /path/to/your.patch
```

Prepare to build the ubuntu package:

```
$ dpkg-source --commit $ dpkg-buildpackage -us -uc
```

In case <tt>dpkg-buildpackage</tt> complains about no upstream tarball found, you can obtain the .orig.tar.gz file with <tt>apt-get source</tt> and copy it to the base folder of your work.

```
$ apt-get source wpasupplicant
```

Finally, update the debian changelog and commit the change locally:

```
$ dch -i $ bzr commit
```

If all looks good, you can push the fix to launchpad and propose merging it into the official package:

```
bzr push lp:~\<your-launchpad-id\>/ubuntu/\<release\>/\<package\>/\<branchname\> bzr lp-open
```

The last command will open the Launchpad page of the remote branch in your browser. There click on the "(+) Propose for merging" link, to get the change reviewed by somebody and included in Ubuntu.

