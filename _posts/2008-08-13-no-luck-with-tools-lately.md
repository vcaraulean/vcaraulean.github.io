---
comments: true
date: 2008-08-13 12:22:28
layout: post
title: No luck with tools lately...
categories: [tools]
---

#### AnkhSVN

A mate came on me with "I found a wonderful SVN client which works from Visual Studio. It's called [AnkhSVN](http://ankhsvn.open.collab.net/)". When I tried it last time it was in version 1.x, ugly & buggy. Having a well working TortoiseSVN I had no regrets uninstalling it. 

Lately it passed a major update. It looks nicer, integrates well in VS and has all functions required for day-by-day work with Subversion. The mate who recommended it said that he found no problems during a week of trying it. 

Ok. Having a refactoring to do that will involve moving few files I decided to give it a try. Moving/renaming files is one of the pain points when working with TortoiseSVN. After committing my changes to repository, the TeamCity server failed to build the project - few files was missing or had unmodified content when it should. AnkhSVN didn't committed all changed files to repository. In VS they was shown as unmodified, but TortoiseSVN show them as changed files. A commit from TortoiseSVN has fixed the problem. 

Still in doubts about it, worth keeping or not...

#### MBUnit Plugin for Resharper

I wanted to run test suite of Rhino.Mocks which uses MBUnit. First try - installed last version of [MBUnit plugin for Resharper](http://code.google.com/p/mbunit-resharper/). It worked nice when I run a single test fixture. But failed to find any tests when I choose "Run Unit Tests" from context menu of a project or solution. Uninstalled.

#### Gallio Automation platform

Still wanted to run the Rhino.Mocks's unit tests, installed [Gallio](http://www.gallio.org/) a bundle that contains among others the MBUnit, a standalone test runner and yet another MBUnit integration into Resharper's Test Runner. Results:

  * Integration with Resharper: didn't discovered all test from test project, cannot run those discovered.
  * Standalone test runner - cannot run test suite, FixtureExecutionException.

The last two tools concerns MBUnit. All I wanted to do is just to run quickly a test suite. I can do it from console, but it's painful and I want to do it from VS and have results there. I can do it with TestDriven.NET - but I don't like it because found it too intrusive in my work environment. I think this is the reason why we haven't moved to MBUnit from NUnit. It's it superior as unit test framework, but weak tooling support make it unusable for us.

& no time now to check what is not working and how to fix it. May be later...

PS: I've seen it one more time: It's very important the very first impression about something to be positive. If somebody tries your software and it blows up or doesn't do what is it about, you fail. But this is another story...
