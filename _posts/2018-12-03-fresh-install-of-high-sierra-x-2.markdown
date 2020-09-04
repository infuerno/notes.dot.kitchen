---
layout: post
title: Fresh install of High Sierra x 2
date: '2018-12-03 22:19:55'
---

1. Copy scripts in ~/ to Dropbox
2. `brew list > brew-apps-2018-08-05.txt`
3. `brew cask list > brew-cask-apps-2018-08-05.txt`
4. Delete bootcamp partition, don't worry about reclaiming the space
5. Prepare USB, reboot into installer, erase disk, install
6. Finder prefs, touchpad prefs, Terminal prefs
7. Change computer name
8. Install Homebrew
9. `brew cask install alfred dropbox spectacle 1password iterm2`
10. Configure Alfred: Features > File Search > Actions > File Selection
11. Sign into Dropbox
12. Go for dim sum while Dropbox syncs
13. Mess around with 1password's new password format and bemoan not realising that each vault has its own sync settings (convert to new format https://support.1password.com/cs/switch-to-opvault/#step-3-convert-existing-vaults)
14. Install ruby: `brew install rbenv` (installs `ruby-build`); `rbenv init`; `rbenv install -l` to list all ruby versions; `rbenv install 2.5.1`; `rbenv global 2.5.1`; check with `rbenv version` (reference https://github.com/rbenv/rbenv)
15. Apply solarized theme to iterm2
16. Clone dotfiles repo; review and copy one by one to ~/
17. Install XCode full via App Store, and open
18. Install vim with powerline using https://coderwall.com/p/yiot4q/setup-vim-powerline-and-iterm2-on-mac-os-x; with `set laststatus=2` to show all the time
19. `brew cask install sublime-text brackets`
20. Install Package Manager for Sublime Text (https://packagecontrol.io/installation)
   - cmd-shift-p : install package : MarkdownEditing
   - cmd-shift-p : install package : MarkdownPreview
   - edit the user preferences file to increase the default wrap-width
21. `brew install nodejs`
22. `brew cask install dotnet-sdk`


### Things I forgot to do
1. Back up .dotfiles
2. Back up 1password vaults - think this may be a wise step before reinstalling next time - or just get a membership!