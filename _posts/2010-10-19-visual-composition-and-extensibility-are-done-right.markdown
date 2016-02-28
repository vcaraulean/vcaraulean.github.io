---
comments: true
date: 2010-10-19 10:28:05
layout: post
title: Visual composition and extensibility are done right when...
categories: [development, silverlight]
---

... when in order to look at some debug data in runtime you’re not writing log messages, not inspecting with debugger your data, not dumping structures to files. Instead, you’re taking the most confortable route, which is write a Widget and show it in the Dashboard of your app. Oh my, that was really fun to discover. 

How to cook it – Silverlight, Castle Winsor and Caliburn Micro. Add a good part of ‘conventions over configuration’. Mixing together you’re implementing the **Concept** – Dashboard and Widgets. After internal mechanics are done (and tested), adding new **Features** (concrete Widgets) are piece of cake.

Additional reading - Ayende about [Concepts and Features](http://ayende.com/Blog/archive/2009/03/06/application-structure-concepts-amp-features.aspx).
