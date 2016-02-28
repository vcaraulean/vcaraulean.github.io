---
comments: true
date: 2011-05-25 16:30:55
layout: post
title: Quick tip - if Git can't find your global config...
categories: [tools]
---

If you’ve put (or want to put) your **.gitconfig** in your home directory (`C:\Users\username\`) and git cannot find it you may try to add a user variable called **HOME** and set it to **%USERPROFILE%**.

See screenshot for details:

![alt screenshot](/../../../../../images/2011/git-env-config.png)

Useful commands:
    
    git config --global -l 
    git config --global -e

First will show your global configuration file, second will try open it to edit.
