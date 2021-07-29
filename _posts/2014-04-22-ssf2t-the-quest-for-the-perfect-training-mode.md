---
layout: post
title: 'SSF2T: The quest for the perfect training mode'
date: 2014-04-22 01:37:29.000000000 +02:00
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
permalink: "/2014/04/22/ssf2t-the-quest-for-the-perfect-training-mode/"
---
Since I started playing ST a few months ago I wanted to have a training mode on MAME like the one available on the Sega Dreamcast port. I collected all the available training mode cheats but none of them convinced me, so I studied all them, fixed some of the glitches and finally end up taking the best of each one to write my own.

The first usable training mode cheat was made by Pasky, he explained it very well in [this comment](http://forums.shoryuken.com/discussion/comment/5141353/#Comment_5141353), the main problem with his cheat is that the stun meter stops working properly after the health bar is recharged and the cheat has to disable stun, so the players never get dizzy. The cheat is interesting because he hooks the game code to make it jump into his own subroutine that refills the health bar for both players. This cheat only recharges the bars after hit damage, but not after throw damage.

After that, there was a [new training mode](http://www.mamecheat.co.uk/forums/viewtopic.php?f=4&t=4103#p13288) cheat made by d9x/dammit. It is way easier because it only recharges the health bar when the dummies are at a certain state (for example after being hit). We can see the value of this state at memory address FF8451 (or 0x400 more for P2). This allows the stun meters to work properly, so characters can get dizzy normally, however it still has some glitches like sometimes when the health is recharged opponent gets hit or pushed back.

In both Pasky and d9x cheats, the health bar can't be empty (if this happens the round will end). This is one of the things that bothered me, because you can't get an idea of how much damage you would do, for example with a 5-hit combo, as the bar is refilled very quickly after every hit or when it reaches a certain value.

Not long ago, [jedpossum published a new training mode](http://forums.shoryuken.com/discussion/comment/8699161/#Comment_8699161), which has the particularity that even after the health bar is empty, the game continues. However it has a few new glitches, like the player's vertical position after a wall throw is sometimes messed up, and there's the K.O. slowdown present when the health bar reaches zero.  
<!--more-->

So I took the ability to reach an empty health bar from jedpossum's cheat, and the memory position to control the player state from d9x cheat and created my own training mode cheat, here is an explanation of the memory values I used:

`ff8451`: player state, a value of 0 means standing, a value of 2 means crouching, a value of 0xE means after a hit, a value of 0x14 means after a throw, etc..  
`ff8478`: the real health value for PL1, range from 0 to 0x90, set to ffff when the round ends.  
`ff847a`: a copy of the health value for PL1, range from 0 to 0x90, it's not modified when the round ends.  
`ff860a`: the health bar meter HUD display value, range from 0 to 0x90.  
`4c94,4a78,4db0,be64e,bebae`: when the round ends, the game engine will take the value from here (0xffff) and change the player's health value at ff8478 with that.  
`FF82F2`: Slowdown value, when set to 0 there's no slowdown.  
`FF84AD, FF84AE, FF84AB`: stun meter, stun counter, dizzy meter

Basically what my cheat does is recharge health when both players are idle (with a "timer" so we can delay it, useful if we want to look at how much damage our last hit has done). It will also recharge health when the round is about to end, in two different ways:

1. when the player is hit or thrown, and his energy is less than 0xf  
2. when the round has actually ended, preventing it to really end

If the last hit (or throw) does less damage than 0xf, the energy will be recharged before the round actually really ends. If the last hit (or throw) does more damage than 0xf the round will "end", but we disable the K.O. slowdown and recharge the health really quick so the round continues and no noticeable slowdown happens.

Here you can see the training mode in action:  
<iframe title="YouTube video player" width="960" height="750" src="http://www.youtube.com/embed/zmpvOeM1CCU?rel=0&amp;hd=1" frameborder="0" allowfullscreen></iframe>

Another thing that the Dreamcast training mode has is that you can force the PL2 dummy to do a certain action, for example always crouch or jump. This is useful for practicing some combos and can be done with macros using mame-rr, but [Ange Albertini](https://twitter.com/angealbertini) suggested me to do it on a mame cheat, so I thought _challenge accepted_, and I also wrote a complementary cheat to control the dummy's repetitive movement... so it's even better than the dreamcast training mode now :)

![super turbo training mode]({{ site.baseurl }}/assets/images/2014/04/ssf2xj-pof-training-mode.png)

This time I've decided to share my [cheat file](https://github.com/poliva/ssf2xj/blob/master/ssf2xj.xml) on github, instead of publishing all the cheats separately, so [grab it](https://raw.githubusercontent.com/poliva/ssf2xj/master/ssf2xj.xml) and if you try my new training mode let me know what do you think in the comments!

Enjoy! :)

**UPDATE: New download bundle with mame-rr, training mode cheat, hitbox viewer, input display and Sako training. All you need to do is put the roms (ssf2.zip ssf2t.zip and ssf2xj.zip in the "roms" folder):**

# [DOWNLOAD](https://drive.google.com/uc?export=download&id=0B1vYN8cImxr9Z2NRdzV0RUI5OXc)

