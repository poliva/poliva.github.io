---
layout: post
title: CD/DVD recording on linux via cmdline
date: 2008-04-28 18:50:25.000000000 +02:00
type: post
categories:
- linux
- minipost
tags: []
meta:
  tags: cdrecord cd dvd burn bchunk linux
permalink: "/2008/04/28/cddvd-recording-on-linux-via-cmdline/"
---
### Kernel Configuration

Device Drivers -- Block devices -- Packet writing on CD/DVD media

### Tools needed

#emerge dvd+rw-tools bchunk

### Copy files

# growisofs -Z /dev/hdb -f -R -r -l -J Folder/

### Burn an ISO

# cdrecord -v speed=8 -sao dev=/dev/hdb ./ubuntu-7.10-rc-desktop-i386.iso

-If using a +RW media, and need to erase first:  
# cdrecord dev=/dev/hdb blank=fast

### Copy an audio CD

# cdda2wav -v255 -D /dev/hdb -B -Owav  
# cdrecord -v speed=8 dev=/dev/hdb -dao -useinfo \*.wav

### Convert bin/cue files into an ISO

# bchunk foo.bin foo.cue foo

