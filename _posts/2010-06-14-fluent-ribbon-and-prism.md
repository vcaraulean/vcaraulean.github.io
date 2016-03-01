---
comments: true
date: 2010-06-14 14:59:53
layout: post
title: Fluent Ribbon and Prism
categories: [community, wpf]
---

I would like to show an example of application that is about how to use [Fluent Ribbon](http://fluent.codeplex.com) control together with [Prism (aka Composite WPF)](http://compositewpf.codeplex.com). Our team is using this combination for some time already; although, with various degrees of success: the Fluent Ribbon is not enough mature (but pretty stable) and my personal relation with Prism is kind of love/hate. The [Castle Winsor](http://www.castleproject.org/container/) is starring as IoC Container of choice.

The code is not something extraordinary by any means. You can find lots of similar samples for other controls in the net. It’s just a sample how to use these libraries together. 

In this example I’ve set next scenario: a module (or another part of application) wants to add new **RibbonTabItem** to existing **Ribbon** without having a direct reference to it. Also, application should support activation/deactivation and removing of Tabs.

The basics are pretty straightforward: pick/write an IRegion, write an adapter to work as a mediator between it and control being extended, profit!..

I’ve decided to use an existing Region to serve as host for tabs - **SingleActiveRegion**. It seems to fit well expected behavior.

Two classes that I wrote are **[RibbonRegionAdapter](http://github.com/vcaraulean/FluentRibbonAndPrism/blob/master/FluentRibbonAndPrism/Infrastructure/RibbonRegionAdapter.cs)** and **[RibbonRegionSyncBehavior](http://github.com/vcaraulean/FluentRibbonAndPrism/blob/master/FluentRibbonAndPrism/Infrastructure/RibbonRegionSyncBehavior.cs)**. The behavior class is used to synchronize the Tabs collection of the Ribbon with Views and ActiveViews collections of the SingleActiveRegion. Adapter serves only to instantiate the region and attach the behavior.

Also, this solution uses Castle Windsor to wire up all things. The **WindsorBootstrapper** class wires up an application based on Prism v2.2 and Castle.

I’ll not publish the the code here, you can "git clone ..." it from [repository on GitHub](http://github.com/vcaraulean/FluentRibbonAndPrism). Or, from the same place, you have the option to [download sources](http://github.com/vcaraulean/FluentRibbonAndPrism/archives/master).

Future plans include few more regions (Backstage, RibbonTabItem) and may be some tests. 

Solution specifics:

  * Prism v2.2 
  * Fluent Ribbon rev. 51299 
  * Castle Windsor 2.1.1 
  * VS 2010 & NET 4.0 

Disclaimer: code is provided “AS IS”. It may contain bugs and not be written in best possible way. I’m not a Prism specialist by any means. Any help is welcome if somebody can improve this code or wants to contribute for a more meaningful example...
