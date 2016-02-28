---
comments: true
date: 2011-12-19 21:36:34
layout: post
title: Hitting the limits...
categories: [general]
---

> When you continuously seek out the limit, you’ll realize that it’s often much higher than you expected. Yes, you can make that screen even simpler than the bare-boned version you’re looking at. Yes, you can trust your employees much more than you imagined. Yes, you can launch without a billing system.
> 
> Once you train yourself to seek out the limit in all endeavors, you’ll get better and faster at correcting the inevitable oversteps, and hit that peak performance.
> 
> But if you never dare venture close to the limit, you’ll find that it’s shrinking all the time. There will be even more people you could possibly offend, even more bells that need whistling, ever more realities of the real world you cannot change.


From [Seven degrees of slip](http://37signals.com/svn/posts/3058-seven-degrees-of-slip).

Lately one of the limits I’ve hit were advanced threading concepts in .NET. It’s an area where I haven’t stepped much aside from some rather simple use cases & basic usage. But recently I had to solve few (non trivial for me) threading puzzles/exercises and today I’ve been hit by IIS showing ‘Deadlock detected’ for one of our Server components. It hurts...

In the same time it clearly shows a limit in my knowledge and experience. So, let’s hit it with an action plan: dig dipper in Threading. Then review those threading puzzles and sort out the deadlock out of the system.

Resources:

  * [Threading in C# by Joseph Albahari](http://www.albahari.com/threading/). It’s a book available online for free
  * [Concurrent Programming on Windows by Joe Duffy](http://www.amazon.com/dp/032143482X/). Paper book and Kindle from Amazon

Both are well recommended on StackOverflow...
