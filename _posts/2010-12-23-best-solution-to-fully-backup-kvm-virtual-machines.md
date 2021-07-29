---
layout: post
title: Best solution to fully backup KVM virtual machines
date: 2010-12-23 16:27:43.000000000 +01:00
type: post
categories:
- linux
tags:
- backup
- kvm
- libvirt
- lvm
- virtual
meta:
  _wp_old_slug: ''
permalink: "/2010/12/23/best-solution-to-fully-backup-kvm-virtual-machines/"
---
Today I found the "[virt-backup.pl](http://repo.firewall-services.com/misc/virt/virt-backup.pl)" script from Daniel Berteaud which is IMHO the best solution to fully backup a libvirt managed virtual machine. The perl script is very flexible and allows for various configurations depending on your VM setup and backup needs, it requires ubuntu packages "libsys-virt-perl" and "libxml-simple-perl" to run.

I use it to backup running virtual machines in this way: first, suspend the virtual machine (for a short while), then create a snapshot of the LVM used as storage by the virtual machine, then resume the virtual machine so it can continue running without almost no downtime (taking a LVM snapshot is very fast!), and after that dump the snapshot to a compressed image file on a remote server (backup storage). To accomplish this just run this command (for example from a cron job):

`
virt-backup.pl --pre --vm=machine1 --backupdir=/mnt/remotenas/ --privatedir --compress --snapsize=5G --debug
`

If you use ubuntu, note that the script needs to be edited to adjust the location of lvcreate and lvremove (in /sbin instead of /usr/sbin).

