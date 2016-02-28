---
comments: true
date: 2008-04-15 08:34:10
layout: post
title: Quick tips & tricks. BDD test naming macro.
categories: [development, tools]
---

This time it's about useful macro that speeds up writing test names. Although we're not using yet BDD development style (BDD is for Behavior Driven Design) I like to give to my test BDD-style names. You know, something like:
    
	[Test]
	public void When_user_pressed_OK_message_should_be_prepared_and_sent()
	{
	}

Writing down these names, when words are separated by underscores is not easy and error prone.

Fortunately, [Jean-Paul S. Boodhoo](http://codebetter.com/blogs/jean-paul_boodhoo/) has posted in his blog an updated macro for formatting test names in BDD-style manner:

  * [BDD Test Naming Macro - Speed Update](http://codebetter.com/blogs/jean-paul_boodhoo/archive/2008/04/14/bdd-test-naming-macro-speed-update.aspx)


Using this macro you can write test name as an usual sentence, having words separated by spaces. After running the macro it will replace all space characters by underscores.

For me it was first macro written for Visual Studio, but after some rambling within VS I have the macro running and working well. Thereafter, found these links than can help to setup and use VS macros:

  * [How To Create and Run Macro in Visual Studio .NET](http://www.helixoft.com/blog/archives/6)
  * [Assigning a Keyboard Shortcut to Macro in VS](http://www.helixoft.com/blog/archives/8)
