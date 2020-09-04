---
layout: post
title: Fresh install of High Sierra
date: '2017-10-17 22:16:48'
---

That time of year again...

![macOS High Sierra Your Mac. Elevated.](https://www.dropbox.com/s/iccxcctszqz2atx/macoshighsierra.jpg?raw=1)

1. Backup everything needing to be kept on both Mac and Bootcamp
2. Download High Sierra from App Store and make a bootable USB
3. Remove Bootcamp partition and reclaim whole drive
4. Reboot and install High Sierra
5. Opt to use File Vault
6. Finder prefs, touchpad prefs, Terminal prefs
7. Dropbox, 1Password, Homebrew, Alfred, Spectacle
8. Log into App Store and install anything still needed
9. Install all system updates
10. Download latest Windows, prepare USB using Bootcamp assistant, install
11. Purchase extra iCloud space and prepare to push all photos online so this is never a headache for reinstalling again
12. Rebuild ghost server and fresh install while we're at it
13. Install ruby: `brew install rbenv` (installs `ruby-build`); `rbenv init`; `rbenv install -l` to list all ruby versions; `rbenv install 2.4.2`; `rbenv global 2.4.2`; check with `rbenv version` (reference https://github.com/rbenv/rbenv)
14. Install ruby gems: `gem install bundler`; check where gems are getting installed with `gem env`; `gem install jekyll` etc etc
15. Install Package Manager for Sublime Text (https://packagecontrol.io/installation)
16. cmd-shift-p : install package : markdownediting
17. cmd-shift-p : install package : markdown preview
18. edit the user preferences file to increase the default `wrap-width`