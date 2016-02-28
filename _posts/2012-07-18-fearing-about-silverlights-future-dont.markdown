---
comments: true
date: 2012-07-18 11:26:29
layout: post
title: Fearing about Silverlight's future? Don't...
categories: [development, silverlight, wpf]
---

Just accept the facts, prepare & migrate to WPF when ready.

When you have something worrying you, the best way to tackle it is to cut it straight and act on it. That's the way to get it out of your head. If not, you're left on the mercy of external factors that may change the circumstances and the problem will go away.

> “If you wait by the river long enough, the bodies of your enemies will float by.” 
> - Sun Tzu

For some time I have a strange feeling about Silverlight. Few years ago it was "the solution" for cross-platform Rich Internet Applications. It was Microsoft's "offre du jour" for Enterprise Software development, offering ease of deployment & update, run anywhere premise and offering a (part of) common development platform for Windows: XAML & .NET.

#### Times are changing

The rapid rise of HTML5 & CSS3 have had a huge impact on client side application development. Reach features of "next web" markup & styling together with explosion of JavaScript popularity (adoption & tooling) makes Web Applications an extremely viable "Rich Client" solution even for Enterprise Software. And sometimes it works nicely for mobile applications too.

Then there is Microsoft's focus on Windows 8 & WinRT. All bets are on the new and shiny Windows 8, Office 2013, Windows Phone 8 & Surface. WinRT is "the new Silverlight", this time without the goal to run on multiple platforms but with a lot of aspirations to support Microsoft's moves in tablet and mobile markets.

#### And now

Looking back at my experience as a Silverlight developer, there is a sensible level of deception and frustration. Missing features, cut-off or half-backed functionality, leaking resources and a failed promise to offer a viable way to develop cross-platform applications. And the worst of all, I think we've lost time working around various limitations of Silverlight in order to get it working properly and comply with our requirements (somewhere between 10 to 15 %%).

The current version of Silverlight is 5. Microsoft is quiet about his future - current releases will be supported, but absolute no information about next versions. Instead, there are plenty of rumors that there will be no Silverlight 6. Microsoft is too busy pushing the Windows 8 and the Silverlight's incarnation on this platform, the WinRT.

#### What do you have to loose? Not too much...

More important is what you can gain by migrating your current Silverlight application to WPF:

  * A normal Windows application, running on all versions of Windows from XP to 8.  
  * The power of WPF & full .NET framework.  
  * Full integration with OS: printing, scanning, file system, services.  
  * Better integration with other applications. MS Office, Reporting tools, you name it.  
  * Touch & Multi-Touch support. You can create applications for Windows 8 that aren't Metro, but are touch friendly.

The main thing you’d probably miss is the ability to run application directly in your browser, without installing it. But if it’s an Silverlight OOB application, you probably need some features not available in his sandboxed environment and running in browser is already out of question.

#### Would it be hard to migrate to WPF? It depends...

If you have a well structured application that follows good practices, it shouldn’t be a hard task to complete. Major steps to migrate a Silverlight application to WPF will be:

  * **Libraries & frameworks**. If you were careful picking those, it now should be a pretty simple task of switching assemblies. Should it be the MVVM framework (Caliburn.Micro or MVVMLight), IoC container (Castle.Windsor) or third-party visual component library (Telerik, DevExpress) - usually they have versions for Silverlight and WPF. In ideal case you will end up just swapping assemblies.  
  * **Porting the source code**. Again, depending on how you've approached development process it may be not so hard to do. It most cases it will be just "copy files" over. But some parts would need to be rewritten, often to simplify or throw away things. I estimate that volume of code to be reviewed/rewritten will be around 15% of overall client side code. Resharper & good old Find-Replace are your friends here. 
  * **Installer & application updates**. Up until now you relied on Silverlight to install & update the application. This part should be redone. ClickOnce is a viable solution, it will serve well your initial requirements to install and update the application. It would take 2-3 days to build, test & make available a decent solution for this problem.  
  * **Rigorous testing after migration**. May be most difficult and time consuming one if you have a large application as there is no tooling support to help with it.

I'm not advocating blindly for immediate migration of every Silverlight application to WPF. If your application have reached his first release, doesn't have a long lifespan and current users are happy - you'll probably be fine sticking with Silverlight. But if you're still in active development or the application is a part of company's strategic products & directions, you should very seriously consider the move.
