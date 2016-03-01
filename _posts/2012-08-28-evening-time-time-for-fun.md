---
comments: true
date: 2012-08-28 21:07:18
layout: post
title: Evening time, time for fun
categories: [general]
---

Or for whatever keeps you entertained...

This evening it started when I looked how to make Windows Live Writer store drafts in my SkyDrive. As usual, started with googling it. Found a [trick with a registry key](http://lehsys.blogspot.ch/2011/04/how-to-change-drafts-and-recent-posts.html) that should point WLW to any folder I want. Nice trick, except it was not working on my PC.

Why a trick that works for other people isn’t working in my machine?! This hooked me up and it ended badly. In about 2 hours I’ve:

  * Poked around WLW’s registry settings  
  * Run [procmon & procexp](http://technet.microsoft.com/en-US/sysinternals) on WindowsLiveWriter.exe 
  * Decompiled Windows Live Writer using [dotPeek](http://www.jetbrains.com/decompiler/)  
  * Got a rough picture of WLW’s code structure and some knowledge of it’s initialization procedure  
  * Found the code that handles custom folder and seen that it should work as the trick described  
  * Learned from code that WLW can run in portable code and [how to do it](http://www.christophdebaene.com/blog/2011/08/20/windows-live-writer-2011-tips/)  
  * Learned how to use Just-In-Time Debugger to [launch an .exe file and attach the Visual Studio’s debugger](http://msdn.microsoft.com/en-us/library/a329t4ed.aspx) from application’s start  
  * Failed to make WLW crash so I can break in debugger  
  * Learned about [setting a function breakpoint](http://msdn.microsoft.com/en-us/library/15d1wtaf.aspx), and that’s useless without symbols  
  * Gave up...

Then I had a glass of Talisker and before giving up completely tried again the trick with registry key...

**It. Worked.**

Moral of the story – stupid, evident errors happen all the time. And, sadly, it takes time to recover. You’re lucky if you’re not under pressure, running to deadline or doing a demo. And you’re really lucky if you can learn new things on the way and keep yourself entertained :)
