---
comments: true
date: 2011-05-31 15:14:27
layout: post
title: Quick tip - Configure TeamCity 6.5 to run a personal build when you've committed to a branch
categories: [tools]
---

This feature is well enough to upgrade your TeamCity to latest 6.5. Of course, if you’re using a DVCS (Git or Mercurial) and use feature branches. As feature branches can be active for a longer time and last few dozens of commits, you want to plug that branch into your Continous Integration process. That’s for sure...

Feature description by JetBrains:

> **Remote run on changes in DVCSs branches**: New build trigger added that watches for commits into Git or Mercurial branches and adds personal build to the build queue when commit detected.

It took me some time to find how it can be enabled. Finally [found it in documentation](http://confluence.jetbrains.net/display/TCD65/Branch+Remote+Run+Trigger). The trick is – **it’s a build trigger**. Just add a new trigger, called **Branch Remote Run Trigger** and enable it. 

You can specify a pattern that will be used to select which branches will be picked up. I’ve specified `*` because I wanted that every branch to be continuously built & tested.

But you you have really long-living branches I would suggest a separate build for it however...
