---
layout: post
title: 'From Evolution to Thunderbird: setting reasonable defaults'
date: 2008-05-29 23:10:57.000000000 +02:00
type: post
categories:
- minipost
tags: []
meta:
  tags: thunderbird evolution MUA mail email
permalink: "/2008/05/29/from-evolution-to-thunderbird-setting-reasonable-defaults/"
---
<p>Having been an Evolution user since a few years back, I recently migrated to thunderbird and had a few "problems". Thanks to <a href="http://www.labutxaca.net">iolanda</a> I was able to fix most of them. I post my thunderbird setup here, so I can remember when I need to setup it again.</p>
<p><strong>Config Editor Settings</strong></p>
<p>Always keep threads, also when changing sort preferences:<br />
<code>mailnews.thread_pane_column_unthreads=false</code></p>
<p>By default sort everything threaded descending by date:<br />
<code>mailnews.default_sort_type=22</code> (22 is threaded, 18 is date)<br />
<code><del>mailnews.default_sort_order=2</del></code> (1 is ascending, 2 is descending) ---
there seems to be a bug here, setting it to 2 does not work and slows down thunderbird!

Do not arrange threads by subject (look at msgid, references, in-reply-to, etc.):  
`mail.strict_threading=true`

Check mail in all folders:  
`mail.check_all_imap_folders_for_new=true`

**Dictionaries**

- [Catalan](http://downloads.mozdev.org/dictionaries/spell-ca.xpi)
- [Spanish](http://downloads.mozdev.org/dictionaries/spell-es-ES.xpi)
- [English](http://downloads.mozdev.org/dictionaries/spell-en-US.xpi)

**Add-Ons**

1) [Dictionary switcher](https://addons.mozilla.org/en-US/thunderbird/addon/3993)  
2) [Disable drag and drop in the folder pane](https://addons.mozilla.org/en-US/thunderbird/addon/4591) (prevents accidentally moving folders)  
3) [FolderCheck](https://addons.mozilla.org/en-US/thunderbird/addon/4936) (only useful if you don't enable check mail in all folders).  
4) [Header Scroll extension](https://addons.mozilla.org/en-US/thunderbird/addon/1003)  
5) [LookOut](https://addons.mozilla.org/en-US/thunderbird/addon/4433) (decode winmail.dat attachments)  
6) [Sync On Arrival](https://addons.mozilla.org/en-US/thunderbird/addon/1396) (Downloads new message headers immediately when there are new messages in a folder)  
7) [ThreadBubble](https://addons.mozilla.org/en-US/thunderbird/addon/5326) (resort the threaded view by date when new mail arrives)  
8) [View Headers Toggle Button](https://addons.mozilla.org/en-US/thunderbird/addon/210)  
9) [ThreadKey](https://addons.mozilla.org/en-US/thunderbird/addon/2299) (Use T/U keys to switch between threaded and unthreaded views).  
10) [Enigmail](https://addons.mozilla.org/en-US/thunderbird/addon/71) (Sign and Encrypt/Decrypt emails using GPG)  
11)[SmtpSelect](https://addons.mozilla.org/en-US/thunderbird/addon/2234) (Switch SMTP server quickly)

