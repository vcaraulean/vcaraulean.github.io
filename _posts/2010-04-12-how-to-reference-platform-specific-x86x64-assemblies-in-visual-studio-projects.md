---
comments: true
date: 2010-04-12 14:04:24
layout: post
title: How to reference platform specific (x86/x64) assemblies in Visual Studio projects
categories: [development]
---

Let’s say you have a third party library that you want to reference from your project. By default platform target in Visual Studio is "**Any CPU**". But the library is compiled for specific target – **x86** or **x64**. When you’re working in a team, have a build server and all of them is in different platforms you’ll start to have issues. Like, project with an x64 referenced assembly working ok on your x64 machine and failing on x86 computers and so on.

For us it was the “**System.Data.SQLite**” – the [ADO.NET provider for SQLite](http://sqlite.phxsoftware.com/). It is provided compiled separately for 32 and 64 bit systems. It was not easy to figure out how to manage references to SQLite provider without making too much changes in our projects and build scripts.

I’ve found an elegant solution for it: manually modifying project files that are using it to add a conditional elements to <Project> node. Look at this code:

	  <Choose>
	    <When Condition="$(PROCESSOR_ARCHITECTURE) == 'AMD64' Or $(PROCESSOR_ARCHITEW6432) == 'AMD64'">
	      <ItemGroup>
	        <Reference Include="System.Data.SQLite, Version=1.0.65.0, Culture=neutral, PublicKeyToken=db937bc2d44ff139, processorArchitecture=AMD64">
	          <SpecificVersion>False</SpecificVersion>
	          <HintPath>..\Lib\x64\System.Data.SQLite.DLL</HintPath>
	        </Reference>
	      </ItemGroup>
	    </When>
	    <Otherwise>
	      <ItemGroup>
	        <Reference Include="System.Data.SQLite, Version=1.0.65.0, Culture=neutral, PublicKeyToken=db937bc2d44ff139, processorArchitecture=x86">
	          <SpecificVersion>False</SpecificVersion>
	          <HintPath>..\Lib\x86\System.Data.SQLite.DLL</HintPath>
	        </Reference>
	      </ItemGroup>
	    </Otherwise>
	  </Choose>

`<Choose> <When Condition= … /> <Otherwise … /> </Choose>` – it’s fun how people are trying to add “behavior” to a such static thing as XML. Well, it works, sometimes…

Detection of the current platform is based on checking of two environment variables: PROCESSOR_ARCHITECTURE and PROCESSOR_ARCHITEW6432 to detect “bitness” of running OS/processes. Details of this you can find in the post of David Wang [HOWTO: Detect Process Bitness](http://blogs.msdn.com/david.wang/archive/2006/03/26/HOWTO-Detect-Process-Bitness.aspx).

Working like a charm for us.
