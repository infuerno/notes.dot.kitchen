---
layout: post
title: Using fastmail.fm to host dot.kitchen email
date: '2015-10-25 07:43:45'
tags:
- hosting
- email
---

I've been meaning to set up email for my own domain for a while. 

## Email address history

After leaving university and losing those email addresses (ma1016@bris.ac.uk, ph@bris.ac.uk) I used hotmail for a couple of years before bailing out and using postmark.net at the back end of 1999 (seems to have since disappeared - [link to old homepage](http://web.archive.org/web/19981212020532/http://postmark.net/); [link to forum chat of the shutdown](http://www.emaildiscussions.com/showthread.php?t=38283)). That was followed by various ISP mail addresses and a brief stint using a [gmx.net](http://gmx.net) address (bit of a pain since the whole interface was in German) before finally settling into gmail. My inbox is flooded with the spam of every newsletter, shopping site, estate agent I've handed my address to in the past few years. I started occasionally using a yahoo email address and the disposable mail boxes they allow you to set up to try and control this a bit better. Keep meaning to set up email on a domain or two I host so...

## Fastmail
I do have a VPS, so I could host my own email server, but that seems a bit over the top on maintenance. After a brief search I initially stabbed at Zoho Mail which has a free plan with some good recommendations. I gave it a shot, but ended up misconfiguring my default email address and then could never seem to change. Further googling showed various dissatisfaction with Zoho. 

Take two. Fastmail has been getting popular, is clean, and although paid, is reasonable.

### Sign Up
They offer email addresses on their own domains as well as allowing you to receive email on your own domain on some plans. So sign up is simple - choose a new email address, password and voila.

### Setup domain
Instructions at: https://www.fastmail.com/help/receive/domains-advanced-setup.html

* Log in to fastmail
* Go to Settings > Advanced > Virtual Domains
* Add domain: `dot.kitchen` and other defaults
* Add alias of `*` as a catch all
* Log in to DNS hoster and add MX, SPF and DKIM settings
* Send a test email **to** the new domain
* Set up a new personality (Settings > Advanced > Personalities) using the new domain (e.g. notes@dot.kitchen)
* Wait TTL from DNS changes
* Send a test email from the new domain (to e.g. a gmail account) and check the headers upon receipt

> Received-SPF: pass (google.com: domain of notes@dot.kitchen designates 66.111.4.221 as permitted sender) client-ip=66.111.4.221;

>Authentication-Results: mx.google.com;

>spf=pass (google.com: domain of notes@dot.kitchen designates 66.111.4.221 as permitted sender) smtp.mailfrom=notes@dot.kitchen;

>dkim=pass header.i=@messagingengine.com