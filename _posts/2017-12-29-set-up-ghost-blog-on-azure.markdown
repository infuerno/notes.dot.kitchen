---
layout: post
title: Set up Ghost blog on Azure
date: '2017-12-29 13:26:31'
---

Ghost can be hosted OR installed on your own server. Every time I upgrade I seem to come a cropper, so here are my notes on spinning up a new server on Azure and getting Ghost installed and set up using a custom domain over HTTPS.

The current hosting notes are here: https://docs.ghost.org/v1.0.0/docs/hosting and recommend Ubuntu 16.04. The actual server set up notes are here: https://docs.ghost.org/v1.0.0/docs/install. These take off after spinning up the new server.

### Provision Ubuntu 16.04 server

1. Log into the Azure portal and choose an appropriate image. Ubuntu Server 16.04 LTS published by Canonical is the only 16.04 offering currently, select the image, select Resource Manager and Create.
2. Enter a name for the server, a username and paste in your public key `pbcopy < ~/.ssh/id_rsa.pub`
3. [Choose a size](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes-general). Since I don't use this much, I went for a "Burstable VM" size B2S Standard for £14.97 a month pro rata (later configured to spin down each night to stay within the Visual Studio subscription free credit rules viz no production servers).
4. Default settings should be fine, configuring Auto-shutdown for 3 am and turning off boot diagnostics
5. Accept T&Cs and start the provisioning
6. Will take approx 5-10 mins to provision the new VM

### Login

1. SSH in via the public IP address: `ssh claire@13.81.108.220`

### Setup server and install Ghost

1. The user specified when creating the VM should already have su privileges, so nothing to do for the first part of the ghost instructions
2. Run `sudo apt-get update` and `sudo apt-get upgrade`
3. Follow the instructions to install Nginx, open the firewall, install MySQL, install node.js and install Ghost-CLI
4. Finally run the ghost installer using `ghost install`. There is a separate page of instructions to explain the various prompts at: https://docs.ghost.org/docs/cli-install#section-prompts (run `ghost setup` to rerun this part if nec). Generally accept the defaults unless there is a good reason not to (e.g. setting up SSL afterwards). 

### Test locally

1. `wget http://localhost:2368` on the server itself should successfully retrieve the index.html page

### Set up for external access

Nginx was configured in the ghost install to proxy requests from localhost:2368 to your chosen domain, but this won't be accessible until the DNS is pointing at the correct IP address and Azure is configured to allow requests through.

1. Update DNS records at your domain provider
2. While this is taking effect, add an entry to your local hosts file to test access e.g. to `/etc/hosts` (remove it later)
3. All Azure VMs created via the GUI are created with a Public IP and a Network Security Group. The Network Security Group controls access to the subnet your VM is provisioned within. To allow access from the internet to port 80 or port 443 (which Nginx is proxying the requests to Ghost on), add an inbound security rule on the NSG for port 443 (and port 80 if yet to configure SSL).
4. Test access e.g. http://notes.dot.kitchen
5. After the DNS propagation has taken effect run `ghost config url [your-ghost-url]` and then `ghost setup ssl` to configure a free Let's Encrypt certificate for your domain and configure Nginx to proxy the ghost install over port 443 instead
6. Test access e.g. https://notes.dot.kitchen

### Configure admin

1. Access the admin panel at [your-ghost-url]/ghost/
2. Enter an admin username and password

### Import previous content

If you've screwed up a previous upgrade like me, head over to the labs page and reimport your .json backup file. If you changed the default theme or had images in the content folder, time to sort this out now too.

