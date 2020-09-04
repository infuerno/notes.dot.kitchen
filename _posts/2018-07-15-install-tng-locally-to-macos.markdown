---
layout: post
title: Install TNG locally to macOS
date: '2018-07-15 17:49:00'
---

## Set up AMP

### References: 
Part 1: https://getgrav.org/blog/macos-sierra-apache-multiple-php-versions
Part 2: https://getgrav.org/blog/macos-sierra-apache-mysql-vhost-apc
Also: https://coolestguidesontheplanet.com/get-apache-mysql-php-and-phpmyadmin-working-on-macos-sierra/

Follow instructions to ensure there is a working Apache, PHP and MySql (or MariaDB) environment.

1. Set default timezone in php.ini

## Install TNG
1. Download most recent version from the [TNG site](http://tng.lythgoes.net) (purchase required)
2. Create new database `mysql -u root -p` e.g. 
```
CREATE DATABASE newdb;
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON * . * TO 'newuser'@'localhost';
FLUSH PRIVILEGES;
```
3. Load readme.html in browser and run through Express install
4. Open site in browser

## Upload gedcom from MacFamilyTree

1. Export GEDCOM file from MacFamilyTree including media **ensuring** to specify Line feed format "Unix"
2. Run scripts to reassign index numbers for FAMS (Family), FAMC (Family), INDI (Individual), SOUR (Sources) types and to inline the notes
3. Run import the first time to import the Custom Event Types
4. Run import again to import the data
![alt](https://www.dropbox.com/s/3jle9y0de52dfr4/Screenshot%202016-12-27%2012.33.22.png?dl=1)
5. Increase max image upload size in php.ini from 2Mb to 10Mb.
6. Import media on the Media > Upload tab

