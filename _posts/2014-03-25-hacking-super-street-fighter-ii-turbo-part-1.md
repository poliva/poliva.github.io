---
layout: post
title: Hacking Super Street Fighter II Turbo (Part 1)
date: 2014-03-25 00:49:00.000000000 +01:00
type: post
categories:
- SuperTurbo
tags:
- mame
- ssf2x
- ssf2xj
- streetfighter
- superturbo
meta:
permalink: "/2014/03/25/hacking-super-street-fighter-ii-turbo-part-1/"
---
In this post I will show how to debug the Super Street Fighter II Turbo ROM in MAME, to create a simple cheat. This will (hopefully) be the first post of a series that will show more advanced  use of the MAME debugger and dig deeper into reverse engineering Super Turbo.
First we need to launch MAME using the `-debug` parameter, this will launch the MAME debugger. You can use the `help` command in the debugger to see the help.
![mame debugger]({{ site.baseurl }}/assets/images/2014/03/mame-debugger.png)

<!--more-->
For this first example we'll create a cheat to change the game difficulty on the fly. To do this, we'll press F5 (or type `go` in the debugger console) and we'll press F2 (the "test switch" key) to access the game's Test Menu. In the test menu, under "System Configuration" we can change the settings of the game, one of those settings is the Game Difficulty, ranging from 1/Easieast to 8/Hardest.

![ssf2xj-system-configuration-menu]({{ site.baseurl }}/assets/images/2014/03/ssf2xj-system-configuration-menu.png)

Now we need to discover what changes in the game's memory when we change that setting. To do this we'll use the debugger's '`dump`' command, that dumps program memory as text. We can find the right syntax for the '`dump`' command using the debugger's command '`help memory`'. It expects a filename as first argument, the start address and the length of the memory portion we want to dump. So we'll put the Game Difficulty setting to 1/Easiest and issue the following dump command: `dump easiest.txt,0,0xffffff`, then we'll set the Game difficulty setting to 8/Hardest and repeat the command: `dump hardest.txt,0,0xffffff`. Now we will use the diff utility to see the difference between both files:
```
$ diff -urN easiest.txt hardest.txt
--- easiest.txt	2014-03-25 00:39:10.038344425 +0100
+++ hardest.txt	2014-03-25 00:39:24.222343789 +0100
@@ -593047,7 +593047,7 @@
 90C960:  0020 0000 0020 0000 0020 0000 0050 0017  . ... ... ...P..
 90C970:  0045 0017 0020 001B 0020 0000 0020 0000  .E... ... ... ..
 90C980:  0020 0000 0020 0000 0020 0000 0043 000D  . ... ... ...C..
-90C990:  0020 0000 0031 0003 0020 0000 0031 001B  . ...1... ...1..
+90C990:  0020 0000 0031 0003 0020 0000 0038 001B  . ...1... ...8..
 90C9A0:  0020 0000 0053 0003 0020 0000 004F 0003  . ...S... ...O..
 90C9B0:  0020 0000 004F 0003 0020 0000 004F 0003  . ...O... ...O..
 90C9C0:  0020 0000 004F 0003 0020 0000 0054 001B  . ...O... ...T..
@@ -1046571,7 +1046571,7 @@
 FF82A0:  0000 0000 0000 0000 0000 0000 0000 0101  ................
 FF82B0:  0000 0000 0000 0101 0000 0000 02C0 0000  ................
 FF82C0:  0000 C000 0000 0000 0000 0000 0000 0000  ................
-FF82D0:  C91C 0000 0000 0000 0000 0100 0000 0000  ................
+FF82D0:  C91C 0007 0000 0000 0000 0100 0000 0000  ................
 FF82E0:  0000 0000 0100 0100 0000 0000 0000 0000  ................
 FF82F0:  0000 0000 0000 0000 0000 0000 0000 0000  ................
 FF8300:  0000 0000 0000 0000 0000 0000 0000 0000  ................
```

We can see two values have changed, one at position 0x90C99D and the other at position 0xFF82D3. The first one is the probably the text displayed on the screen, while the second one is probably the position in memory where the difficulty value is stored, as we can guess it ranges from 0 (Easiest) to 7 (Hardest). To confirm our guess we'll launch the Memory View window in MAME's debugger, to do this just press CTRL+M and the Memory View window will appear. Type in the memory address of our previous finding and then switch the Game Difficulty in the System Configuration Menu to see if the memory value at 0xFF82D3 changes when we change the difficulty.

![ssf2xj memory]({{ site.baseurl }}/assets/images/2014/03/ssf2xj-memory.gif)

It appears that our guess was right, and we have found the place in memory where the Game Difficulty setting is stored. Now let's do the MAME cheat to be able to change this setting on the fly during gameplay, so we can adjust the difficulty while playing against the CPU.

The MAME cheat system XML format is explained in the comments on the [src/emu/cheat.c](http://mamedev.org/source/src/emu/cheat.c.html) file inside MAME source code. We have to create a cheat file inside MAME's "cheat" folder named the same as the ROM zip file but with xml extension, so for Super Street Fighter 2X (Japanese version of Super Turbo), the filename will be "ssf2xj.xml". First we open the `mamecheat` tag, then we open the `cheat` tag and add the description for example "Select Difficulty". Then we will use the `parameter` tag to let the user chose the difficulty using `item` values from 0 to 7. Then we'll use a temporary variable `temp0` to store the current value of the Game Difficulty when the cheat starts (`script state="on"`), we'll use this variable to restore the value to the user's setting when the cheat ends (`script state="off"`). While the cheat is being run (`script state="run"`) we'll just overwrite the memory value at address 0xFF82D3 with the `param` value selected in the cheat menu. The final cheat code looks like this:

[https://gist.github.com/10100976](https://gist.github.com/10100976)

So now using our newly created cheat we can adjust the difficulty during gameplay against the CPU by accessing the cheats menu (press TAB on MAME, it must be started with `-cheat` option):

![ssf2xj cheats menu]({{ site.baseurl }}/assets/images/2014/03/ssf2xj-cheats-menu.png)

Enjoy :)

**UPDATE:** As [jedpossum points out](http://forums.shoryuken.com/discussion/comment/8743741/#Comment_8743741), the following memory map of the CPS2 would have helped to decide what needed to be changed instead of guessing (thank you!):

```
CPS2:
0x000000 - 0x3FFFFF Main Program
0x400000 - 0x40000A Encryption (aka the battery memory on pheonixed roms it's 0xFFFFF0 - 0xFFFFFA)
0x618000 - 0x619FFF Shared ram for the Z80 aka tells what sfx or music to play.
0x660000 - 0x663FFF Network Memory
0x900000 Start of Graphic memory (can change with each game)
	
ST:
0x900000 - 0x903FFF Palette
0x904000 - 0x907FFF 16x16
0x908000 - 0x90BFFF 32x32
0x90C000 - 0x90FFFF 8x8
0x910000 - 0x913FFF 16x16 mainly hud and character names on select screen
	
0xFF0000 - 0xFFFFFF Main Memory
```
