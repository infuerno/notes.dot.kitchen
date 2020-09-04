---
layout: post
title: Upgrading Ghost
date: '2015-10-11 21:50:30'
---

I created this site using an image from Digital Ocean with Ghost already installed. Its on version 0.6.4 and I notice there is later version available (0.7.1). Time to upgrade.

## References

http://support.ghost.org/how-to-upgrade/

1. Back up content by exporting json file using settings > labs > export
2. Remote onto box
3. Download latest `curl -LOk https://ghost.org/zip/ghost-latest.zip`
4. Unzip `unzip ghost-latest.zip -d ghost-latest`
5. Remove core directory, index.js, *.md and *.json: `sudo rm -rf core sudo rm index.js && sudo rm *.md && sudo rm *.json`
6. Copy in files from latest: `copy -R ~/ghost-latest/core/ .`, `cp ~/ghost-latest/index.js ~/ghost-latest/*.json ~/ghost-latest/*.md .`
7. Copy latest default theme `cp -R ~/ghost-latest/content/themes/casper content/themes`
8. Update persmissions `chown -R ghost:ghost *`
9. Upgrade dependences `npm install --production`
10. Restart `service ghost restart`

Successfully upgraded at the same time as writing this!