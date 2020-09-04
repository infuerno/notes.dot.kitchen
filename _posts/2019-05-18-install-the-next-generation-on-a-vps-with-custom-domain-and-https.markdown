---
layout: post
title: Install The Next Generation on a VPS with HTTPS
date: '2019-05-18 22:17:00'
---

[The Next Generation (TNG)](https://www.tngsitebuilding.com) is (amongst other things) software to host a family tree online on your own website. There are other options for doing this, some of them free, but TNG seems to be about the best of this category and is reasonably priced ($32.99 for the latest full version). The developer, Darrin Lythgoe, releases patches and new releases regularly and there is an active community, forums etc.

The installation instructions are **very** comprehensive but only cover the approach more common a few years back, when people used to host their sites on shared hosting plans with FTP rather than shell access. VPSes are cheap and offer greater control.

These are the steps I took to set up the latest version of TNG (v12.1) on a $5 VPS hosted by [Vultr](https://www.vultr.com/?ref=8095441-4F)\* (1GB RAM, 25GB SSD, 1CPU, 1GB bandwidth), running over HTTPS. I already use the VPS to run this blog, so certain things were already installed (e.g. Nginx, MariaDB). I had installed older versions of TNG previously, but my interest in genealogy goes in spates and the most recent version was on a server long spun down and deleted.

\* Affiliate link, get $50 free hosting (I get $25 free hosting).

### Domain

To host your own website, you need your own domain. While it is possible to get a free domain (e.g. obscure TLDs like .tk (Tokelau) offer free domains - guess what, they are used by lots of spam sites) most registrars charge about $10 a year and you can often find a bargain for the first year. I have used [gandi.net](https://www.gandi.net/en) for a number of years and have never had any issues. You only need one domain and can then use subdomains for different things. I own a number of domains (can't let them go!) and will be using `furney.org` to host TNG.

Once you have a domain and a VPS up and running, add a DNS record to point your domain (or subdomain) to the VPS's IP address. This may take a few hours to propagate.

Shown below is `@` which resolves `furney.org` to the IP address given. Further down another A record (not shown) with name `www` ensures the site is accessible via `www.furney.org` as well (seems to be the done thing to set up this once ubiquitous subdomain).

![image](https://www.dropbox.com/s/ri2kd32sj86ts1n/gandi-dns-for-furney-org.png?raw=1)

### Set up Nginx

I already had Nginx installed and up and running, so I just needed to add a virtual site for the new domain. The OS on the VPS is Ubuntu 16.04 LTS. (The PHP install instructions linked further below also cover off Nginx.)

<!--kg-card-begin: markdown-->
1. After purchase, follow the instructions to download the latest zip of the TNG software locally
2. Upload to the server (execute on local machine): `scp tngfiles121.zip claire@furney.org:tngfiles121.zip` (will upload to the specified user's home directory e.g. `/home/claire` in this case)
3. Unzip to new directory in web root: `sudo unzip tngfiles121.zip /var/www/furney.org`
4. Create new config in `/etc/nginx/sites-available` by copying default: `sudo cp default furney.org`
5. Edit the nginx config just created. If this is the first (default) site being set up, edit the main part with the correct details (has directives for the default server), otherwise delete that and uncomment and edit the section at the bottom for additional configurations. Mine ends up looking like this:

```
server {
	listen 80;
	listen [::]:80;

	root /var/www/furney.org;

	# Add index.php to the list if you are using PHP (we are)
	index index.html index.php;

	server_name furney.org www.furney.org;

	location / {
		# First attempt to serve request as file, then
		# as directory, then fall back to displaying a 404.
		try_files $uri $uri/ =404;
	}
}
```

1. Add a symlink to sites-enabled: `sudo ln-s /etc/nginx/sites-available/furney.org /etc/nginx/sites-enabled`
2. Restart nginx: `sugo service nginx restart`
3. The `readme.html` is the place to start and should now be browsable if everything in nginx was configured correctly e.g. [http://furney.org/readme.html](http://furney.org/readme.html)

![TNG Installation Instructions](https://www.dropbox.com/s/e0xjhe3v2it13lq/tng-installation-instructions.png?raw=1)

<!--kg-card-end: markdown-->
### Set up SSL

Now is as good a time as any to get SSL set up. The TNG software has a public interface for displaying the family tree as well as an admin interface for managing the configuration / installation. To manage the installation via the web interface, you'll need to enter a username and password and its just good practice to not have these details flying around the internet unencrypted.

Let's Encrypt issue certs for **free** for 90 days and can be set up to automatically renew on a cron.

Since I'm running multiple SSL sites with different certs and have run into difficulties in the past, the first thing I did (as recommended [here](https://www.biggleszx.com/2017/02/setting-up-lets-encrypt-with-multi-vhost-nginx-on-/)) was to set up site wide secure SSL settings and prepare a default self-signed root configuration: [https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-in-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-in-ubuntu-16-04).

Following this I set up SSL for the domain hosting TNG, `furney.org`: [https://hostadvice.com/how-to/how-to-setup-lets-encrypt-with-nginx-on-an-ubuntu-18-04-vps-or-dedicated-server/](https://hostadvice.com/how-to/how-to-setup-lets-encrypt-with-nginx-on-an-ubuntu-18-04-vps-or-dedicated-server/)

<!--kg-card-begin: markdown-->
1. Add the repo to the certbot tooling: `sudo add-apt-repository ppa:certbot/certbot`
2. Update the package list: `sudo apt update`
3. Install certbot: `sudo apt install python-certbot-nginx`
4. Backup the nginx config before certbot edits it with the SSL details (in case everything goes pear shaped): `sudo cp /etc/nginx/sites-available/furney.org /etc/nginx/sites-available/furney.org.backup-20190516`
5. Get the certificate: `sudo certbot --nginx -d furney.org -d www.furney.org`

![Certbox output](https://www.dropbox.com/s/sx7mflfy9sxm19e/furney-org-enable-https.png?raw=1)

1. Test

Bonza! No more "Not Secure":

![SSL enabled successfully](https://www.dropbox.com/s/h9vaddfma6td0rh/furney-org-ssl-enabled.png?raw=1)

<!--kg-card-end: markdown-->
### Install TNG

There are two version of the installation instructions. "Begin Here" which outlines briefly what needs doing and may be all that is required. When things don't work first time (do they ever?), more in depth instructions arev available for each of the Begin Here steps.

One frustration I have found with the current installation instructions are the assumption you are using point and click tooling rather than the shell. Below is what I needed to do to get this up and running:

<!--kg-card-begin: markdown-->
#### Step 3 Set permissions

Note 664 for the file permissions didn't work for me. 755 for the folder permissions also didn't work. Sometimes the devil wears prada.

1. Update ownership: `sudo chown -R claire:claire /var/www/furney.org/`
2. Set file permissions: `sudo chmod 666 adminlog.txt config.php genlog.txt importconfig.php logconfig.php mapconfig.php pedconfig.php subroot.php whatsnew.txt`
3. Set folder permissions: `sudo chmod 777 photos histories documents headstones media gedcom gendex backups`
<!--kg-card-end: markdown-->
### Install PHP

At this point I discovered php is not yet installed. These instructions were helpful: [https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-in-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-in-ubuntu-16-04)

<!--kg-card-begin: markdown-->
1. Install php and the preprocessor required for nginx: `sudo apt-get install php-fpm php-mysql`
2. Secure the installation: `vi /etc/php/7.0/fpm/php.ini`, uncomment `cgi.fix_pathinfo=1` and update to `cgi.fix_pathinfo=0` (don't second guess what file someone wants).
3. Restart: `sudo service php7.0-fpm restart`
4. Configure nginx to use the PHP preprocessor by editing the server conf: `sudo vi /etc/nginx/sites-available/default` (I actually added it to `furney.org` conf)

    	location ~ \.php$ {
    		include snippets/fastcgi-php.conf;
    		fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    	}
    
    	location ~ /\.ht {
    		deny all;
    	}

1. Check nginx config: `sudo nginx -t`
2. Restart nginx: `sudo service nginx restart`
3. Finally, set the configuration settings in `/etc/php/7.0/fpm/php.ini` as per the TNG instructions:

    file_uploads = On ; already ok
    error_reporting = E_ALL & ~E_NOTICE ; change from default
    extension=php_mysqli.dll ; uncomment
    upload_max_filesize = 10M ; change from default 2M
    output_buffering = 4096 ; suggestion is On, but I left this

1. Finally restart php again: `sudo service php7.0-fpm restart`
<!--kg-card-end: markdown--><!--kg-card-begin: markdown-->
#### Step 4 Rename two folders

    sudo mv backups [suitablyesotericnameforbackups]
    sudo mv gedcom [similarilyunguessablenameforgedcom]

Note for later from the installation instructions: If you need to do [this manually], make the changes within TNG as well after the installation is complete by going to the Admin/Setup area at [http://www.yoursite.com/genealogy/admin\_setup.php](http://www.yoursite.com/genealogy/admin_setup.php) (if you're reading this from your web site, click here: admin\_setup.php. From there go to "Setup" and click on "General Settings" and "Import Settings".

<!--kg-card-end: markdown--><!--kg-card-begin: markdown-->
#### Step 6 Establish a connection to your database

MariaDB (fork of MySQL) was already installed, but I needed to create a database, create a user and give this user permissions on the database. This is a standard step for many of these kind of installs and I always forget the exact syntax.

1. Log onto the sql server: `mysql -u root -p` (enter password when prompted)
2. Create database: `CREATE DATABASE tngdb;`
3. Create user: `CREATE USER 'tngusr'@'localhost' IDENTIFIED BY 'passw0rd'`
4. Give user full permissions on the new database: `GRANT ALL PRIVILEGES ON tngdb . * TO 'tngusr'@'localhost';`
5. Reload permissions: `FLUSH PRIVILEGES;`

Obviously, make it a strong password!

1. Fill in the database details on the form on the TNG readme.html and cross your fingers. With a prevailing wind you'll see the message "Information saved, connection verified!"

![Connection established](https://www.dropbox.com/s/znnx2qppf4tgwk0/furney-org-database-connection-established.png?raw=1)

#### Step 7 Create the database tables

1. Fill in the collation for the database (affects how string are sorted, I used `utf8_general_ci` since it'll be mainly English) and click the button to save and create the tables.
<!--kg-card-end: markdown-->

Once you've created the database tables, you can go to the admin page and check the `backups` and `gedcom` folders that were renamed have been correctly recognised and sort out if necessary.

### Template

Get in your time machine and choose a template from the page: [http://lythgoes.net/genealogy/templates.php](http://lythgoes.net/genealogy/templates.php)

### Check the diagnostics

Go to the Setup \>\> Diagnostics page ([https://furney.org/admin\_diagnostics.php](https://furney.org/admin_diagnostics.php)) and check for orange or red.

For GD graphics library: `sudo apt install php7.0-gd`

