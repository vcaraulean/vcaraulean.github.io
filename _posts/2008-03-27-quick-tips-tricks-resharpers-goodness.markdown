---
comments: true
date: 2008-03-27 15:06:58
layout: post
title: Quick tips & tricks. ReSharper's goodness.
categories: [development, tools]
---

I have shown today few tricks with [ReSharper](http://www.jetbrains.com/resharper/index.html) to one of my teammates. I think it worth to be shared/stored in the blog.

1. View the code used from a referenced assembly

Very often you want to see how a referenced assembly is used in your project. We can use dedicated tools to work this out (for example, NDepend) or you can just click on a project in solution explorer, expand "References" node, select an assembly, right-click and select "**Find Dependent Code**". You will see a nice "Find Results" window with all places where this referenced assembly is used. 

Same way, you can analyze project dependencies in a multi-project solution. I think you will be rewarded in future if your presentation assemblies will not depend from a project with database stuff. 

2. Setting a keyboard shortcut to run a unit test

I'm sure that any keyboard ninja knows it. Do you want to learn a bit of kung-fu? Go to keyboard settings configuration in Visual Studio (Tools > Options... > Environment > Keyboard), find a command named "**ReSharper.UnitTest_ContextRun**" and assign a shortcut to it. I hang it to "Ctrl+1".

Now, when you're editing a class containing unit tests, you can just press your newly created shortcut and ReSharper will run the tests: if you're inside a test method (method marked with [Test]) only the current test will run; if you're somewhere in the class, but outside of a test method, R# will run all tests from this class.
