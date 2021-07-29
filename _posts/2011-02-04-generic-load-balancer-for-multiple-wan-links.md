---
layout: post
title: Generic load balancer for multiple WAN links
date: 2011-02-04 02:35:44.000000000 +01:00
type: post
categories:
- linux
tags:
- ADSL
- balancing
- iproute2
- link
- multipath
- provider
- router
- routing
- wan
meta:
  yourls_tweeted: '1'
permalink: "/2011/02/04/generic-load-balancer-for-multiple-wan-links/"
---
So you have two or more ADSL lines and want to use them all?  
... or maybe you're stealing your neighbor's wifi and you have more than one network available?  
... or you have cloned your SIM card and want to use multiple 3G connections simultaneously?

You can easily setup your Linux box to route multiple connections using iproute2, no matter how many WAN links you have! I have made a script to automate the process, it also comes with an optional failover watchdog which will monitor all the WAN links and automatically disable those which fail, re-enabling them when the connection is back.

Configuration is simple -one column for each WAN link-, you just need to have a separate interface for each link. If you want to use only one physical interface I recommend using a different VLAN on each WAN link. Edit the script to configure it as follows:

```
# # Specify each WAN link in a separate column, example: # # In this example we have 3 wan links (vlanXXX interfaces) attached to a single # physical interface because we use a vlan-enabled switch between the balancer # machine and the ADSL routers we want to balance. The weight parameter should # be kept to a low integer, in this case the ADSL line connected to vlan101 and # vlan102 is 4Mbps and the ADSL line connected to vlan100 is 8Mbps (twice fast) # so the WEIGHT value in vlan100 is 2 because it is two times faster. # # WANIFACE=" vlan101 vlan100 vlan102" # GATEWAYS=" 192.168.1.1 192.168.0.1 192.168.2.1" # NETWORKS=" 192.168.1.0/24 192.168.0.0/24 192.168.2.0/24" # WEIGHTS=" 1 2 1" # # quick formula to calculate the weight: (LINKSPEED/MINSPEED)\*NUM\_LINKS # # If you don't want to use vlans, you should then use a separate physical # interface for each link. IP aliasing on the same interface is not supported. #
```

The script will set up the default route to be a multipath route, this will balance routes over the multiple WAN links and cache them depending on the destination address (so often used sites will always be sent over the same link). The weight parameters can be adjusted for each link in case you don't have the same speed on each.

**Download: [generic-balancer.sh](/archives/files/generic-balancer.sh)**

Enjoy! ;)

