---
comments: true
date: 2012-05-30 16:20:30
layout: post
title: Greatest feature of TortoiseHg and GitHub for Windows is still missing it
categories: [tools]
---

[GitHub for Windows](http://windows.github.com/) was released recently and it’s a huge advancement in usability of [Git SCM](http://git-scm.com/) in Windows. Until not so far existent tooling has been giving you the command line integration and hopefully some (crappy) basic UI . [Git Extensions](http://code.google.com/p/gitextensions/) has made lately great progress but I still preferred the command line for routine tasks. 

This lack of good tooling around Git was the reason why I’ve promoted Mercurial in my company when we’ve migrated some of our projects to DVCS. [TortoiseHg](http://tortoisehg.bitbucket.org/) is great for working with Mercurial repositories and is evolving steadily adding new features & improving stability.

There is one killer feature that I absolutely love in TortoiseHg and I’m using it daily. I do most of source-control operations from the command line/PowerShell. But whenever I want I can launch the TortoiseHg’s UI using simple commands that would bring me right where I need to:

  * **thg** or **thg log**: the log window
  * **thg ci**: commit dialog
  * **thg shelve**: shelve dialog

So, with a couple of keystrokes I can launch (almost any) dialog for a Mercurial repository in current folder, do quickly the job and close it away to be back to my lovely Console. That’s a true time saver and integrates very well in my workflow.

This is the feature I miss in GitHub for Windows: launch it from command line, accept command line parameters and show me the screen I’ve asked for. Isn’t it a great idea?

Probably it’s not easy to add this feature as for now GHfW is deployed using ClickOnce and it has enough quirks and limitations. But who knows what the future brings...

