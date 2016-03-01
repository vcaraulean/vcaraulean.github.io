---
comments: true
date: 2010-10-13 15:37:54
layout: post
title: 'Multi-targeting with Project Linker by example: Castle.Core for Silverlight 4'
categories: [castle, silverlight]
---

In this post I would try to explore multi-targeting of a .NET project/solution using [Project Linker](http://msdn.microsoft.com/en-us/library/ff921108(PandP.20).aspx) tool. My main driver is desire to target one of our internal projects to Silverlight. I’ve picked [Castle.Core](http://github.com/castleproject/Castle.Core) to serve as an example, since his Silverlight build is now broken and (from what I’ve seen) there is no convenient way to work with this project in Visual Studio while targeting Silverlight. 

For more details and screenshots of Project Linker, please refer to [this MSDN article](http://msdn.microsoft.com/en-us/library/ff921108(PandP.20).aspx).

Let’s go, step by step instructions:

 1. Go to Extension Manager in VS 2010 and search for Project Linker. 
 2. Install Project Linker extension 
 3. Create new solution, say Castle.Core-SL 
 4. Add to this solution the existent Castle.Core project (targeted for NET 4 Client Profile) 
 5. Add to solution new Silverlight 4 project, named Castle.Core-SL 
 6. Click on Castle.Core-SL project and from context menu choose “Add project link…” 
 7. In “Select Source Project” dialog choose the Castle.Core project, click OK. 
 8. To verify that projects are linked correctly, in VS Project menu choose “Edit links”: Source Project should be set to “Castle.Core”, and Target Project – “Castle.Core-SL”. 
 9. In order to trigger real linking process (Project Linker is observing file changes in source project) I have a trick for you. Go to Castle.Core project, then select all folders containing source files (Components.DictionaryAdapter, Core and DynamicProxy) and click on “Exclude from project” in context menu. Then, after enabling “Show all files” options, add them back. Here Project Linker will kick in and will link all added source files to Target Project. 
 
Then, we can do the same for project containing unit tests.

In case of Castle.Core there is another manual intervention required: filter out CastleBuild.snk file from being linked. To do that you have to open project file for your project and find an attribute in it called ProjectLinkerExcludeFilter. Add to the end of that value the string “;\.snk”. After that you should “Ad As Link” the key file manually.

If you want to “re-sync” both projects, you have to repeat only the 9th step.

Now, you’ll have your Castle.Core targeted to Silverlight with all source files from original .NET project. You’re now should be confortable enough exploring the code in Visual Studio and, why not, contribute...
