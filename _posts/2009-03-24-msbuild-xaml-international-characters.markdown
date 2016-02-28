---
comments: true
date: 2009-03-24 17:00:50
layout: post
title: MSBuild, XAML & international characters
---

More a note to myself, because I'm sure, will bump again in this issue later.

I've edited some strings in our XAML files to correct some french words. Someone pointed that I'm not playing well with french-specific characters (you now, the é, è, à, ...). After committing my changes our build server went down with a strange error:

	error MC3000: 'Invalid character in the given encoding. Line 58, position 47.' XML is not valid.

Hmm. On my machine VS has built and run it without any problems. Compiling from command line - same error.

**Problem**: Compiler launched from command line cannot process the source file because of international characters that I've just added. 

**Solution**: Save source file explicitly in UTF-8 encoding to preserve international characters.

**How To**: 

  * open the problematic file in Visual Studio.
  * on the **File** menu click "**Advanced Save Options**"
  * from "Encoding" combo select "**Unicode (UTF-8 ...**"
  * click OK.

You're set. Commit to please the build server and rest of the team waiting for green.
