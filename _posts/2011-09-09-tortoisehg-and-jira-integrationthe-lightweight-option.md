---
comments: true
date: 2011-09-09 10:29:08
layout: post
title: TortoiseHG and JIRA integration - the lightweight option
categories: tools
---

You can integrate JIRA and TortoiseHG Workbench at two levels:

  * Only highlight the issue in your revision details and allow quickly open it in browser
  * Connect to your JIRA and show the list of issues with all filters, view details and pick issues to include in your commit.

Now we’ll focus on the easiest option to implement – displaying issue number in commit message and open it in browser.

Required settings:

  * Open **Issue Tracking** category in Settings dialog of TortoiseHG
  * Set properties to
    * **Issue Regex**: \b\w{3,7}-\d+\b
    * **Issue Link**: [http://your.jira.server.com:8080/browse/{0}](http://your.jira.server.com:8080/browse/{0})
  * Ignore the rest of settings for now

See the screenshot, it should look like this:

![alt screenshot](/../../../../../images/2011/tortoisehg-jira-1.png)

Now if your commit message in revision details will contain a code of an Issue (or what will be detected as code) it will be highlighted as a hyperlink and clicking it will open a browser with your Issue details in JIRA.

![alt screenshot](/../../../../../images/2011/tortoisehg-jira-2.png)

I’m trying to get a deeper integration working, unsuccessfully so far. When it will be done will publish a follow-up post...
