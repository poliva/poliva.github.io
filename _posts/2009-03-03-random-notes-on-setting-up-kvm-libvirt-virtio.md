---
layout: post
title: random notes on setting up KVM + libvirt + virtio
date: 2009-03-03 04:47:02.000000000 +01:00
type: post
categories:
- linux
tags:
- kvm
- libvirt
- lvm
- virtio
meta:
permalink: "/2009/03/03/random-notes-on-setting-up-kvm-libvirt-virtio/"
---
This is a collection of notes I took while setting up a virtual machine host which has several guest virtual machines running on Ubuntu 8.10.

**1) Create a logical volume to install the guest**

```
$ sudo lvcreate -v -n phq\_mail -L 80G vg0 /dev/md1 Setting logging type to disk Finding volume group "vg0" Creating directory "/etc/lvm/archive" Archiving volume group "vg0" metadata (seqno 6). Creating logical volume phq\_mail Creating volume group backup "/etc/lvm/backup/vg0" (seqno 7). Found volume group "vg0" Creating vg0-phq\_mail Loading vg0-phq\_mail table Resuming vg0-phq\_mail (254:5) Clearing start of logical volume "phq\_mail" Creating volume group backup "/etc/lvm/backup/vg0" (seqno 7). Logical volume "phq\_mail" created
```

Remember you can view all your logical volumes using <tt>lvdisplay</tt>

**2) Creating a network segment to separate the servers from the rest of the network** (the clients will use routing through the host to access the server).

```
$ vim ~/pofhq-servers.xml \<network\> \<name\>default\</name\> \<uuid\>e81218cf-6d5a-4a6f-8af8-b2d5b77947be\</uuid\> \<bridge name="virbr%d" /\> \<forward/\> \<ip address="192.168.25.1" netmask="255.255.255.0"\> \<dhcp\> \<range start="192.168.25.2" end="192.168.25.30" /\> \</dhcp\> \</ip\> \</network\> $ virsh net-define pofhq-servers.xml Network pofhq-servers defined from pofhq-servers.xml $ virsh net-create pofhq-servers.xml Network pofhq-servers created from pofhq-servers.xml $ virsh net-autostart pofhq-servers Network pofhq-servers marked as autostarted $ rm ~/pofhq-servers.xml
```

This will create the file pofhq-servers.xml into <tt>/etc/libvirt/qemu/networks</tt> and link it to autostart folder.

Optionally, if you don't want to use the 'default' network segment, you can delete it:

```
$ virsh net-undefine default $ virsh net-destroy default
```

This will automatically delete de default-network.xml file (and autostart symlink if present) on <tt>/etc/libvirt/qemu/networks</tt>.

**3) Installing the guest operating system using virtio for best virtual machine network and disk performance**

We will start by letting _virt-install_ create the default VM template for us:

```
$ sudo virt-install -n phq\_mail -r 1024 -f /dev/vg0/phq\_mail -c ubuntu-server.iso --accelerate --vnc --noautoconsole -v Starting install... Creating domain... 0 B 00:00 Domain installation still in progress. You can reconnect to the console to complete the installation process.
```

Right after this, we will stop the VM and edit it's configuration manually:

```
$ virsh shutdown phq\_mail $ virsh dumpxml phq\_mail \> phq\_mail.xml $ virsh undefine phq\_mail $ virsh destroy phq\_mail $ vim phq\_mail.xml
```

Make the following modifications:

- Boot from CD:

```
\<os\> \<type\>hvm\</type\> \<boot dev='cdrom'/\> \</os\> \<disk type='file' device='cdrom'\> \<source file='/home/pau/ubuntu-8.10-server-i386.iso'/\> \<target dev='hdc' bus='ide'/\> \<readonly/\> \</disk\>
```

- Use virtio for the hard disk:

```
\<disk type='block' device='disk'\> \<source dev='/dev/vg0/phq\_mail'/\> \<target dev='vda' bus='virtio'/\> \</disk\>
```

- Use virtio for the network:

```
\<interface type='network'\> \<mac address='00:16:36:7a:7b:7c'/\> \<source network='pofhq-servers'/\> \<model type='virtio'/\> \</interface\>
```

Save the file, and create again the virtual machine with the new config:

```
$ virsh define phq\_mail.xml Connecting to uri: qemu:///system Domain phq\_mail defined from phq\_mail.xml $ virsh create phq\_mail.xml Connecting to uri: qemu:///system Domain phq\_mail created from phq\_mail.xml $ virsh autostart phq\_mail Connecting to uri: qemu:///system Domain phq\_mail marked as autostarted
```

Install normally, and then change the boot option to 'hd' to boot from normal hard disk again when installation has been finished (if needed, use shutdown/undefine/destroy and define/create/autostart again after finishing the installation).

**4) If the guest VM you have installed is Ubuntu**, remember to install `acpid`, for the VM to shutdown cleanly.

**5) Useful documentation:**

- [http://wiki.libvirt.org/page/Virtio](http://wiki.libvirt.org/page/Virtio)
- [http://wiki.libvirt.org/page/Tips](http://wiki.libvirt.org/page/Tips)
- [https://help.ubuntu.com/8.10/serverguide/C/libvirt.html](https://help.ubuntu.com/8.10/serverguide/C/libvirt.html)
- [http://ubuntuforums.org/showthread.php?t=1026006](http://ubuntuforums.org/showthread.php?t=1026006)
