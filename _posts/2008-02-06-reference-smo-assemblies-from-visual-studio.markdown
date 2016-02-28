---
comments: true
date: 2008-02-06 14:10:31
layout: post
title: Reference SMO assemblies from Visual Studio
categories: [development, tools]
---

Just a note to myself...

If you want to use SQL Server Management Objects (SMO) in your .NET applications you should reference assemblies from "C:\Program Files\Microsoft SQL Server\90\SDK\Assemblies". This is described in almost all articles about how to work with SMO (for example [this one](http://www.yukonxml.com/articles/smo/)). 

But if that folder doesn't exist or required files are missing, they are 2 ways to solve that:

  * Extract required files from GAC
  * Install an additional component for SQL Server: Software Development Kit

Second choice was easier to do for me. So, I just downloaded [SQL Server 2005 Express Edition with Advanced Services SP2](http://msdn2.microsoft.com/en-us/express/bb410792.aspx) and installed the required SDK.
