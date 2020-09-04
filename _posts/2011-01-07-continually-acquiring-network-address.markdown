---
layout: post
title: Continually 'acquiring network address'
date: '2011-01-07 18:55:34'
---


Saviour.

> It is important when using a router that “ICS” is not enabled on any connection in any of the networked computers. Usually if unable to access the DHCP server the router to get an address, Windows would default to allocating a 169.254.x.x address. Has this PC previously been able to connect to the internet? The reason I ask is that the TCP/IP protocol may have been damaged.
> 
> It wouldn’t do any harm to carry out this procedure anyway to repair the TCP/IP:
> 
> Open a Command Prompt window “Start > Run”, type CMD and click OK.
> 
> At the prompt, type:
> 
> netsh interface ip reset resetlog.txt
> 
> and press Enter note the spaces in the command line. The original prompt will reappear, exit the Command Prompt window and reboot the PC.

via [Forums – continually ‘acquiring network address’ – PC Advisor](http://www.pcadvisor.co.uk/forums/index.cfm?action=showthread&threadid=225458&forumid=18).


