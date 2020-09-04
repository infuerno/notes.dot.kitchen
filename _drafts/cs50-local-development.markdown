---
layout: post
title: CS50 local development
---

The recommendation is to use the online IDE at [https://ide.cs50.io](https://ide.cs50.io).

For offline development, there is a docker cs50-ide image documented at [https://cs50.readthedocs.io/ide/offline/](https://cs50.readthedocs.io/ide/offline/), but this didn't seem to work for me and comments on the site are that this is **no longer supported or maintained**.

Alternatively the various tools and libraries can be installed locally, with the benefit being that you can code on your local machine in a text editor of your choosing, set up as you like and of course work offline if required.

Most tools are installed using `pip` the package manager for `python`. Python 3 is required, which is the default for the latest mac OS (Catalina), but can alternatively be installed with homebrew using `brew install python`. Follow the instructions after install to ensure its on the path etc.

<!--kg-card-begin: markdown-->
1. Install `libcs50` from [https://github.com/cs50/libcs50/releases](https://github.com/cs50/libcs50/releases)  
2. Download release  
3. Unzip, `cd` into folder  
4. `sudo make install`  
5. Test by compiling any C program which includes `cs50.h` e.g. `clang -o hello hello.c -lcs50`
2. Set up `virtualenv` and `virtualenvwrapper`
  1. `pip install virtualenv`
  2. `pip install virtualenvwrapper`
  3. Add `source virtualenvwrapper.sh` to `.zshrc` (or equivalent)
3. Create a virtual environment which by default will install the various libraries etc to `~/.virtualenvs/cs50`  
5. `mkvirtualenv cs50`
4. Activate the virtual environment  
7. `workon cs50` (`deactivate` when no longer required)
5. Install tools to check and submit code:  
9. `pip install style50`  
10. Test `style50 hello.c`  
11. If there is an error running style50, potentially resolve by installing libmagic `brew install libmagic`  
12. `pip install check50`  
13. Test `check50 cs50/problems/2020/x/hello` or run locally using `check50 --local cs50/problems/2020/x/hello`  
14. `pip install submit50`  
15. Test `submit50 cs50/problems/2020/x/hello`
<!--kg-card-end: markdown-->