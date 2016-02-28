---
layout: post
title: "Using Caliburn.Micro with async/await"
date: 2013-07-15 11:21
comments: true
categories: 
---
This post will give an overview of how you can use async/await features from C# 5.0 with [Caliburn.Micro (CM)](http://caliburnmicro.codeplex.com/). If you've found an error or want to complete the content please let me know in the comments. A sample application demonstrating async/await interactions in CM [is available on GitHub](https://github.com/vcaraulean/CaliburnMicro.AsyncDemo).

## Setting it up
To use Tasks and async/await features you need the latest version of Caliburn.Micro. Support for Tasks was introduced in version 1.5, so you'll need a version higher than that. And, of course, you need .NET Framework 4.5 or higher. All examples in this post were tested in a WPF application targeting .NET 4.5 framework. However, same code will be probably valid for Silverlight, WP8 and Windows8 apps.

<!-- more -->

#### Create and configure new project to test the concepts
 - Create new WPF 4.5 application
 - Install Caliburn.Micro from nuget. You'd better choose the `Caliburn.Micro.Start` package as it has everything required to run a Caliburn application (boostrapper, container, main screen)
 - Change App.cs to correctly bootstrap with Caliburn, [here is how to do it](http://caliburnmicro.codeplex.com/wikipage?title=Nuget)

## Async Actions
The syntax for attaching actions in XAML will remain unchanged:
    
    <Button x:Name="RunTask1Async">
    <Button cal:Message.Attach="RunTask1Async">

When declaring action's method we can specify that we want to handle it asynchronously:

~~~
    public async void RunTask1Async()
    {
        Log("RunTask1Async - starting a 4 second task");
        await Task.Delay(4000);
        Log("RunTask1Async completed");
    }
~~~

And that's all you need to execute an action asynchronously (if you have to execute a Task).

## Async handlers for Screen events and overrides

CM offers few handy methods that allow to connect to Screen's lifetime events. Some method overrides with async signatures:

~~~
	protected override async void OnInitialize()
	{
		Log("OnInitialize - starting a 7 seconds task");
		await Task.Delay(7000);
		Log("OnInitialize completed");
	}
    protected override async void OnActivate() { }
    protected override async void OnViewLoaded(object view) { }
    protected override async void OnViewAttached(object view, object context) { }
	...
~~~

To note here that even if the method in base class is not marked `async`, our overrides declare them with `async` modifiers and it runs fine.

Similarly, you can handle Screen's events the same way how you're handling any .NET event in asynchronous way:

~~~
	{
		// ...
		ViewAttached += OnViewAttachedEventHandler;
	}
	private async void OnViewAttachedEventHandler(object sender, ViewAttachedEventArgs args)
	{
		Log("OnViewAttachedEventHandler- starting a 1 second task");
		await Task.Delay(1000);
		Log("OnViewAttachedEventHandler completed");
	}
~~~

	
## IHandleWithTask interface
This is a new addition to CM, declared as:

~~~
    public interface IHandleWithTask<TMessage> : IHandle
    {
        Task Handle(TMessage message);
    }
~~~

It's extremely useful when you want to handle a particular message using a Task and off-load application's main thread.

An example:

~~~
	public class Message { }

	public class MessageHandler : IHandleWithTask<Message>
	{
		public MessageHandler(IEventAggregator eventAggregator)
		{
			eventAggregator.Subscribe(this);
		}

		public async Task Handle(Message message)
		{
			await Task.Delay(3000);
		}
	}
~~~

## Convert coroutines to Tasks
This can be done easily by using an extension method:

~~~
	public async void WrapIResultInTaskAsync()
	{
		await new SimpleCoroutine().ExecuteAsync();
	}
~~~

## "async void" or "async Task"
A great question will be: should I use `async void` or `async Task` as a return value of action methods, screen's events and overrides? Since you can do both and the code will compile and run mostly fine, let's see what [Stephen Cleary](http://blog.stephencleary.com/) writes about it in [Best Practices in Asynchronous Programming](http://msdn.microsoft.com/en-us/magazine/jj991977.aspx) article:

> You should prefer async Task to async void. Async Task methods enable easier error-handling, composability and testability. The exception to this guideline is asynchronous event handlers, which must return void. This exception includes methods that are logically event handlers even if they’re not literally event handlers (for example, ICommand.Execute implementations).

I will join this recommendation. Actions are event handlers at their core, normally they are executed as "start and forget". If something happens (an exception), it will be handled at application level. For the case when you want to handle any exceptions locally, you may want to create an override which returns an `async Task`. Also, this will allow calling same method action from another part of the program or write a unit test to verify an asynchronous process.

## References
 - [Caliburn.Micro. 1.5.0 release notes](http://devlicio.us/blogs/rob_eisenberg/archive/2013/03/18/durandal-1-2-0-and-caliburn-micro-1-5-0-released.aspx)
 - ["Best Practices in Asynchronous Programming" by Stephen Cleary (MSDN Mag)](http://msdn.microsoft.com/en-us/magazine/jj991977.aspx)
 - ["Coroutines are dead. Long live Coroutines." by Marco Amendola](http://marcoamendola.wordpress.com/2012/10/16/coroutines-are-dead-long-live-coroutines/)
 - [Async/Await FAQ](http://blogs.msdn.com/b/pfxteam/archive/2012/04/12/async-await-faq.aspx)
 
This is pretty much it. If you've found an error or may complete this post with additional info, let me know in the comments.
