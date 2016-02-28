---
comments: true
date: 2010-09-29 16:16:35
layout: post
title: Houston, we have a problem. Unit tests in Silverlight
categories: [development, silverlight]
---

Going deeply into Silverlight development I’ve hit a bump that have destabilized me. 

I’m at the point where I want to begin write unit tests for a Silverlight app. 

Until this moment, NUnit, Resharper and TeamCity were working formidably well in this regard, covering all my needs: I’ve been productive writing tests and tools were at the hand to pick them up & run. That’s when targeting .NET Framework.

If we’re back to Silverlight, major unit test frameworks are not ready. Silverlight support is either announced or present in some experimental state. And if even I’ll found a way to **write my tests**, Resharper and TeamCity will not be able to **run them automatically**, requiring manual tweaks or installing additional things. 

Looking deeper for a solution...
