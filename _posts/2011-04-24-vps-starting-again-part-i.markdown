---
layout: post
title: 'VPS :: Starting again :: Part I'
date: '2011-04-24 01:24:43'
---


123systems is turning out to be ultra slow trying to get the backend of wordpress to refresh. Not sure if its the network or the server. Just bought a new VPS run by Alvotech in Germany. Came installed with a (very) minimal Debian install. Just going to note down the steps to get it up and running, since I should have done this before. Following some notes on [this blog](http://jblevins.org/log/ubuntu-vps#mysql) to start.

Allowing normal user to sudo, used the command (as mentioned in the /etc/sudoers file) to edit it, and my default editor appears to be nano. Wonder how I change that to vim?

Next is to get DNS working. Need to install apache, webmin and bind.

> sudo apt-get install apache2
> 
> wget [latest webmin deb package]
> 
> sudo dpkg –install [latest webmin deb package]
> 
> sudo apt-get install bind9

Now configure bind using these instructions: [http://www.beer.org.uk/bsacdns/](http://www.beer.org.uk/bsacdns/ "http://www.beer.org.uk/bsacdns/").

Have already configured the domain for this setup, just need to change the IP address to the new servers. Except this time I tried setting up the whole resilience deal using a free DNS for a secondary nameserver. everydns.net doesn’t except new registrations anymore (having been taken over by DynDNS), another popular one had also been bought out by DynDNS, tried Zoneedit but couldn’t understand the interface, now using PointHQ but no idea if I have the correct setting in there or not. If it isn’t working, I’ll configure another day with afraid.org. My primary nameserver is at least working for now.

Install php and test its working with the test.php file (restart apache a couple of times).

Start installing software..


