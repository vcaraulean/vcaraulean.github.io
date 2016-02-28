---
comments: true
date: 2011-09-21 17:13:10
layout: post
title: Cultural differences...
categories: [development, testing]
---

Today I’ve been helping a coworker to bootstrap new project. Nothing fancy: ASP.NET site hosting WCF services, few POCOs and Entity Framework in Code First mode to persist them all. 

Interesting moment came when he wanted to see if his service actually works. He quickly created another project (Windows Forms) to try connect to newly created service. When I’ve seen that I’ve suggested that a unit test project would be easier and more useful, he preferred to go his route and added buttons, large text block and some plumbing code to take service’s response to the screen.

I’m not blaming here (as he is rather new to .NET platform), but his approach has few disadvantages versus verifying a service using unit tests:

  * it’s slower to create a minimally meaningful solution: a crappy screen with a button “connect” and large text box to see results? I’d prefer a test with an assertion and may be some console output  
  * it’s slower to analyze results: you have to scan the screen to see/analyze replies. I’d prefer that a test assertion would take care of that.  
  * it’s error prone: you have to rely on human input to test it. I’d prefer an automated approach.  
  * it’s not repeatable nor scalable: what if you have second service? 10 methods in service?. I’d create new test fixture and refactor existing code, not UI.  
  * ...

... lot’s of things that are wrong with this approach. 

Even if it’s a short living spike, new project, new feature – whatever needs a proof of code in working state – **I’d prefer to write a test for it**. And I’m not talking here about rigorous Test-Driven-Development approach. It’s more like **acceptance testing at service or component boundaries**, only to be sure that it works as expected. And creating an UI just to see that a service works – it sounds wrong to me. But those are “cultural differences”, I think...

PS: By “cultural differences” I mean past experience, habits, education, professional environment and so on...
