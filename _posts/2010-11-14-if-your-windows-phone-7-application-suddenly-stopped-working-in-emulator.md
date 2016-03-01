---
comments: true
date: 2010-11-14 21:36:56
layout: post
title: If your Windows Phone 7 application suddenly stopped working in emulator...
categories: [wp7]
---

... so it isn’t launching anymore and displaying splash screen infinitely, or application launches then stops, and you have done only “usual” development, be cool... Wp7 is a new platform. We, early adopters, are here to catch them, report and hope it’ll be fixed in vnext.

Until then, **check that you haven’t changed the name of assembly**. Also, a symptom for this is this exception in output:

> A first chance exception of type 'System.Collections.Generic.KeyNotFoundException' occurred in mscorlib.dll

TODO: figure out where wp7 issues are reported/recorded, see if isn’t there then create an issue for it. It’s probably a bug in emulator.
