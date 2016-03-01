---
layout: post
title: "Diving deeper in Reactive Extensions, concurrency"
date: 2012-10-09 22:04
comments: true
categories: [rx, wpf]
---
I've started using [Reactive Extensions, Rx](http://msdn.microsoft.com/en-us/data/gg577609.aspx) library without digging too much in it's internals. Had a case where I wanted to filter a stream of events and decided I'd try something new for me, Rx. But as it always happens with powerful and flexible tools, starting with few superficial articles and examples from the Web without diving deeper in the concepts is a pretty bad idea.

This time I've been bitten by *concurrency* issues.
<!-- more -->
And the best resource I've found so far is the [Introduction to Rx](http://www.introtorx.com/) ebook by [Lee Campbell](http://leecampbell.blogspot.co.uk/). It costs only around a buck on Amazon and it's available for free from the site.

I've jumped straight to the [Chapter 4, Concurrency](http://www.introtorx.com/Content/v1.0.10621.0/15_SchedulingAndThreading.html) and for surely will pass without a hurry over the rest of the book. This chapter has an nice check list about best threading options for some scenarios:

### UI Applications
- The final subscriber is normally the presentation layer and should control the scheduling.
- Observe on the DispatcherScheduler to allow updating of ViewModels
- Subscribe on a background thread to prevent the UI from becoming unresponsive
  - If the subscription will not block for more than 50ms then
    - Use the TaskPoolScheduler if available, or
    - Use the ThreadPoolScheduler
  - If any part of the subscription could block for longer than 50ms, then you should use the NewThreadScheduler.

### Service layer
- If your service is reading data from a queue of some sort, consider using a dedicated EventLoopScheduler. This way, you can preserve order of events
- If processing an item is expensive (>50ms or requires I/O), then consider using a NewThreadScheduler
- If you just need the scheduler for a timer, e.g. for Observable.Interval or Observable.Timer, then favor the TaskPool. Use the ThreadPool if the TaskPool is not available for your platform.

----

And by the way, on author's blog I've found another interesting series of articles: [Responsive UIs in WPF - Dispatchers to Concurrency to testability](http://leecampbell.blogspot.co.uk/2009/01/responsive-uis-in-wpf-dispatchers-to.html). That's something I'd be reading after sorting out concurrency issues from my code.
