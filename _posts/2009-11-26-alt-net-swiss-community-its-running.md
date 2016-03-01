---
comments: true
date: 2009-11-26 13:36:04
layout: post
title: ALT.NET Swiss community - it's running!
categories: [community, wpf]
---

Yesterday I have participated at my second meeting of [ALT.NET](http://altdotnet.org/) community in Switzerland. The [ALT.NET Swiss group](http://www.altdotnet.ch/) was running for some time, but I've discovered it recently, after being contacted by Frédéric SCHÄFER from [OCTO Technology](http://www.octo.com). He started the group and has been organizing meetings in Lausanne and Geneva. Many thanks Frédéric and keep up a good job!

First meeting I attended was about WPF and Model-View-ViewModel pattern implementation. It was held by Frédéric SCHÄFER. He took a simple WPF application and slowly modified it to use MVVM pattern. That got us better separation of concerns and maid application more testable.

[The yesterday's meeting](http://www.altnetfr.org/2009/11/19/alt-net-suisse-5-wpfmvvm-geneve/) was for me a special one, I was one of the speakers.

I've talked briefly about how the MVVM pattern is used with WPF, showed a "classical" implementation of it and after that presented some improvements over that. The initial ViewModel had INotifyPropertyChanged implemented and used ICommand to respond to UI actions initiated by view. For two properties and a command there was a lot of "ceremony" - code being there just to comply with implementation requirements. At the end of the session, by using two frameworks([BLToolkit](http://bltoolkit.net/) and [Caliburn](http://caliburn.codeplex.com/)) , I've managed to reduce the volume of the code by the half while maintaining initial functionality. Why is this important? Shorter code means less place to make a mistake, hide a bug, it's faster to read and understand. I will write a separate blog post about my solution.

The other speaker was [Daniel Vaughan](http://danielvaughan.orpius.com/), the author of [Calcium SDK](http://www.calciumsdk.net/). He spoke about Calcium SDK, which is a set of additional services and modules built on top of the Prism (Composite WPF) and few Visual Studio templates to make creation of composite application easier. After that, he talked about compile time validation of WPF's binding expressions. This, being a real headache for WPF developers, was addressed by Daniel by using template engine backed into Visual Studio - T4. [His solution](http://www.codeproject.com/KB/codegen/T4Metadata.aspx) is very interesting. He generates some metadata classes with static properties and after that uses them in binding expressions. I'm still not sold on the whole idea, but I should spend some time playing with it. But presentation was nice, thanks Daniel!

It was a great evening. It's always interesting to meet people with different set of skills and experience, you can look at known problems from other angles, getting suggestions or criticism. Or you just can learn something new.

Special thanks to [Atif Aziz](http://www.raboof.com/) and [Cargill International SA](http://www.cargill.com/worldwide/switzerland/index.jsp) for hosting the session.

Update: [published the code on GitHub](http://github.com/vcaraulean/Demo.MVVM).
