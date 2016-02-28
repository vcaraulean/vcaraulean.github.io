---
comments: true
date: 2012-07-19 17:06:48
layout: post
title: Migrating from Silverlight to WPF, a case study
categories: [silverlight, wpf]
---

#### A case study on porting an application that uses [Caliburn.Micro](http://caliburnmicro.codeplex.com/) and [Telerik’s visual components](http://www.telerik.com/) from Silverlight to WPF

I wrote that [nobody should fear an eventual migration from Silverlight to WPF](/blog/2012/07/18/fearing-about-silverlights-future-dont/). But I wanted as well to work on a “proof of concept” and try to find what difficulties may arise in a such migration and how easy is to actually do it. And, preferable, do it in a small project before taking on big targets.

I’ll use [Caliburn.Micro.Telerik](https://github.com/vcaraulean/Caliburn.Micro.Telerik) as my test ground. It’s a library aimed to [integrate Caliburn.Micro and Telerik’s component suite for Silverlight](/blog/2011/06/05/integration-of-caliburn-micro-and-teleriks-silverlight-controls/). And as I’ve been getting requests to port conventions and few helpers to WPF I’ll use it as my playground.

Silverlight solution contains a Silverlight library and two Silverlight applications to test conventions and other stuff. Both applications together contains more than 10 views with corresponding view models, so it should provide a fairly relevant impression about how easy is to migrate from Silverlight to WPF.

### Step 1

Restructure solution to allow cohabitation of two different versions. In this case, I want to keep both versions, so I’ll be copying files from one project to another. Duplication, but at this moment I don’t want the overhead of keeping two target frameworks with a common codebase.

### Step 2

Creating the destination point – a .NET library or WPF application:

  * Create new Class Library targeting .NET 4 framework or WPF Application with .NET 4 Client profile.  
  * Add Caliburn.Micro using Nuget  
  * If it’s a class library, add references to WPF’s assemblies: PresentationCore, PresentationFramework, System.Xaml & WindowsBase  
  * Add references to Telerik’s assemblies. I’ve picked ones with same names as in corresponding Silverlight project. 

### Step 3 

Getting the application canvas and infrastructure code right. This includes Caliburn’s bootstrapper, the main view & all stuff to get application running. There is one difference, the main view in WPF should be a Window, while in Silverlight it was a simple UserControl.

### Step 4

Migrating your views & view models. In the case of Caliburn.Micro.Telerik examples project I’ve had to just copy the files from Silverlight to WPF project. Then it compiled and run just fine.

### Step 5

Cleanup. 

### Conclusions

Telerik have done a pretty good job on keeping their Silverlight and WPF components in a very similar shape and at a very high compatibility level between both platforms. I hope that it doesn’t diminishes any of advantages/facilities/features of either platform.

Migrating a Caliburn.Micro application from SL to WPF is a piece of cake. Despite few minor differences in infrastructure code between WPF & Silverlight versions of WPF, you’re pretty much left with move files, cleanup namespaces and Run. View models are ported without any issues, however you may encounter few with the views.

As this was a small project, porting is very straightforward and relatively simple. In a real application with lots of stuff and moving pieces you will have some more work to do. Particularly when you have some Silverlight specific code or where you’re working around platform’s limitations. Take for example mouse events. To get a “double click” you have to write custom code. Middle click doesn’t exist in Silverlight. Printing is different, file system interactions too. Communication layer (WCF & service references) will need a review. There’s are no RIA services in WPF (I may be wrong here). The list may go on further, but I’ll stop here. I hope to have much more info to share about migration process as I’ll probably will take part in moving a much larger application from Silverlight to WPF.

That’s all. I hope my experience will help someone else to go over the fear and burden of porting a Silverlight application to WPF.

PS: you can have a look at final result, get sources and compare both versions on [GitHub](http://github.com/vcaraulean/Caliburn.Micro.Telerik).
