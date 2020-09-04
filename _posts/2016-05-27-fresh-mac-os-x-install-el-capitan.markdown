---
layout: post
title: Fresh Mac OS X Install of El Capitan
date: '2016-05-27 07:33:02'
---

Feels about that time of the year to clear out the accumulated drift and get back to a clean slate. My last install was shortly after Yosemite came out, I installed it, hated the new look and feel and went straight back to Mavericks. At that point I had most things in Dropbox and had used Kitchenplan for the last couple of installs. Over the last few months that started to seem more of an overhead than a help. Homebrew lets you install things relatively quickly, and setting up ruby, python etc is fun to do again and worthwhile remembering the procedure. At some point I also downloaded some .dotfiles and found these all I really needed to kickstart my environment.

This time I'm going to keep things fairly simple, but backed up from the start including:

1. Source controlled .dotfiles
2. Put any activation keys etc into 1Password
3. Set up something for photos which doesn't include using iPhoto
4. Use source control more for project work / jekyll site, and get better routines for updating this more regularly and in a more automated way

### Get ideas

* http://simplicitybliss.com/post/125580713412/the-fresh-mac-install
* http://brettterpstra.com/2013/03/26/the-first-apps-on-my-new-mac/
* http://blog.bmannconsulting.com/mavericks-brew-cask

### Pre install steps

1. Export any photos from iPhoto to Box and sync (includes exporting using Phoshare, tiding into folders using Hazel and deduping using a free app on the app store)
2. Tidy up home directory and ensure anything not installed into the Dropbox folders and not needed, moved into Dropbox, copied onto the NAS.
3. Ensure 1Password file is safely in Dropbox
4. Record apps installed via homebrew
5. Record apps installed to Application folder and ~/Applications folder
6. Think if there is anything on Bootcamp which I need to tidy up first? Shelfset anything checked out?

### Prepare OS X

1. Download El Capitan from the app store
2. Burn to USB key as per instructions: http://mashable.com/2015/10/01/clean-install-os-x-el-capitan/#1g8b6EHjVEq0
3. Reboot from USB and follow installation
4. Set up trackpad
5. Run Bootcamp Assistant to create a windows partition and install windows
6. Terminal: Preferences > General > Pro; Profiles > Shell > When the shell exists; Shell > Use Settings as Default
7. Install [homebrew](http://brew.sh) (installs dev tools)
8. `brew cask install dropbox`, configure
9. `brew cask install 1password`, download keychain via dropbox online, configure
9. `brew cask install alfred`, change theme, activate
10. `brew install spectacle`, allow access, start at login
11. Make dock smaller, automatically hide and show
12. Clone bing-wallpaper repo and set up
13. `brew install Caskroom/versions/sublime-text-dev`, activate
14. Finder preferences > Sidebar > show home




