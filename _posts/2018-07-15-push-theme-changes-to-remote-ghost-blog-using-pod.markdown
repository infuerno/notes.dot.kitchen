---
layout: post
title: Deploy theme changes from local development Ghost to remote Ghost using pod
  (git push deploy)
date: '2018-07-15 18:43:16'
---

A git remote can be set up to push local changes straight from a local git repo to a server via ssh. (The remote directory must be a git repo itself, potentially a "bare" one.) Using pod, additional scripts can be run remotely following the push. This is necessary to copy the files into position and restart Ghost. 

Pod itself is a git push deploy tool for pushing node.js apps, but that doesn't stop us using it to git push deploying anything.

## References

* Older article with variation on (and inspiration for) this approach: https://jonathanmh.com/node-js-ghost-theme-development-and-deployment/
* Pod: https://github.com/yyx990803/pod
* Use ghost-cli programatically: https://docs.ghost.org/docs/using-ghost-cli-programatically

## Server setup
1. On the remote ghost server install pod: `npm install -g pod`
2. Pod's working directory must be completely accessible by the user you will be "git pushing" with. Either create this in a place which already has access (`/home/[user]/pod`) OR create somewhere logical and give full access to your user
    i. `sudo mkdir /opt/pod`
    ii. `sudo chown [user]:[user] /opt/pod`
5. Run pod to initialize your chosen directory: `pod`
6. Create an "app" to hold your remote files (basically your remote git repo): `pod create [name-of-theme]`
7. Finally follow the instructions linked above to allow your user to use ghost-cli programatically by setting up passwordless sudo

## Local setup
1. Initialize a git repo inside your theme directory (if not already) e.g. in [ghost-install]/content/themes/[name-of-theme]: `git init`
2. Add remote to point to your server using ssh e.g. `git remote add deploy ssh://me@1.2.3.4/opt/pod/repos/[name-of-theme].git`
3. Test initial push: `git push deploy master`. Files should get pushed to the pod directory on the server - pod will complain about the non-existence of node's `app.js` but this can be safely ignored.

## .podhook to restart Ghost
A `.podhook` file overrides the default pod behaviour and allows running custom commands on the server following the push.  

1. Add a `.podhook` file to the local theme directory and add commands to copy the files from pod's working directory to the theme directory within ghost. e.g.
```
sudo rm -rf /var/www/ghost/content/themes/[name-of-theme]
sudo rsync -rv --exclude=.git --exclude=node_modules --exclude=.podhook --exclude=.gitignore /opt/pod/apps/[name-of-theme] /var/www/ghost/content/themes
sudo chown -R ghost:ghost /var/www/ghost/content/themes/[name-of-theme]
ls /var/www/ghost/content/themes/[name-of-theme]
cd /var/www/ghost
ghost restart --no-prompt
```
2. Commit and push.

![BOOM sign in comic style | designed by Vexels](https://www.dropbox.com/s/vhr8hoisw6gqcd6/Screenshot%202018-07-15%2019.42.51.png?raw=1)

