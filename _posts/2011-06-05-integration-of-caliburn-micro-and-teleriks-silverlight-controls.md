---
comments: true
date: 2011-06-05 20:51:45
layout: post
title: Integration of Caliburn.Micro and Telerik's Silverlight controls
categories: [community, silverlight, tools]
---

I’ve wrote already about how to [use Telerik’s RadTabControl with Caliburn.Micro](/blog/2010/12/11/teleriks-radtabcontrol-and-caliburn-micro/) and profit from Caliburn’s conventions. That post is still getting considerable attention in my blog and also there are people are asking in Caliburn’s discussion list about such kind of integration. So, I’ve decided to spend some of my time and provide a more coherent experience in this domain. Rob Eisenberg [suggested to create a dedicated project](http://caliburnmicro.codeplex.com/discussions/256418) for it and, why not, to create a NuGet package. That sounded very interesting for me as it would allow me to scratch another itch – the [NuGet](http://nuget.org) package manager. We’ve been using it in our projects with great success but I have never explored the part with creation and publication of packages.

### How do you get it

[Caliburn.Micro.Telerik source code on GitHub](https://github.com/vcaraulean/Caliburn.Micro.Telerik). Feel free to pull the code & use only parts you need. There are two main classes, you can copy&paste them in your projects. If you want to contribute, I’d appreciate a push request with new conventions, better code samples or bug fixes. Report issues if you’ve found any...

[Caliburn.Micro.Telerik as a NuGet package](http://nuget.org/List/Packages/Caliburn.Micro.Telerik). “Add Library Package Reference ...” from Visual Studio & start using it. My package has a dependency on Caliburn.Micro.1.1.0 so you can just reference it and Caliburn will be pulled in automatically.

### What’s in

First of all, **conventions**. I’ve put inside conventions we’ve been using in our projects and plan to add more. So far there are conventions for:

  * RadTabControl 
  * RadBusyIndicator 
  * RadDateTimePicker (affects also RadTimePicker and RadDatePicker) 

Then you have a basic **implementation of IWindowManager**, the RadWindowManager class. Nothing fancy, pretty basic stuff. And I plan to extend it to offer also an unified interface to Telerik’s custom dialogs like Confirm, Alert and Prompt.

And third, there are **two sample projects**, one showing how to use conventions and other makes use of RadWindowManager.

### How to use conventions

Add a line in your Bootstrapper’s Configure() method to enable conventions:

	public class AppBootstrapper : Bootstrapper<IShell>
	{      
		protected override voidConfigure() {
		// ...

	        TelerikConventions.Install();
	    }
	}

Check it out and let me know if you found it useful or how it’s possible to make it better...
