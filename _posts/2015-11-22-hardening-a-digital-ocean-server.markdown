---
layout: post
title: Securing a digital ocean server
date: '2015-11-22 22:46:37'
---

After initial provisioning of a new Ubuntu server, the following tutorials show steps required to make the server more secure.

* https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04
* https://www.digitalocean.com/community/tutorials/additional-recommended-steps-for-new-ubuntu-14-04-servers

### Hardening steps

* Create alternative user
* Set up ssh login
* Prevent root ssh login
* Set up firewall and enable
* Configure timezone
* Configure NTP

### Maintenance steps

* `sudo apt-get update`
* `sudo apt-get upgrade`
* `sudo apt-get dist-upgrade`
* `sudo update-grub`
* Select the latest kernel in the control panel
* Power off and power on
