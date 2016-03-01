---
comments: true
date: 2012-06-10 16:02:33
layout: post
title: 'New & shiny Windows Azure: publishing a website using Git'
categories: [tools]
---

After [taking the tour](/blog/2012/06/08/new-shiny-windows-azure-taking-the-tour/) of Windows Azure, the first thing I wanted to try is the [Web Site hosting in Azure](https://www.windowsazure.com/en-us/home/scenarios/web-sites/). As Microsoft is offering a free trial for 90 days and a free extension to 12 months after the trial, you can host up to 10 small web sites for free for more than 1 year! With various options for publishing it makes Azure a no-brainer to host small personal sites and experimental stuff.

### Publishing a web site to Azure using Git

Here are the steps required to configure & publish your local repository to Azure.

#### Configure Git publishing on Azure. 

It’s one time operation where you need to provide a username and password for your git repositories. You’ll be asked for it for it when you’ll try to configure Git for your first site.

#### Create your web site in Azure

![alt screenshot](/../../../../../images/2012/azure-git-1.png)

Provide a name for your site, then I’ll take few seconds to create an instance for it.

#### Configure the Git repository for your site

  * Click on the name of your site to open it’s management console  
  * Click on “Set up Git publishing”

![alt screenshot](/../../../../../images/2012/azure-git-2.png)

In a few seconds your site will have a git repository provisioned.

#### Add new remote to your local Git repository

Open the folder with your local git repository in a command line/PowerShell window and type:

	git remote add azure https://vcaraulean@helloworld-test.scm.azurewebsites.net/helloworld-test.git

Here the “azure” is the name of your remote repository and the link is the address of your repository hosted in Azure.

#### Push your website to newly added remote in Azure

	git push azure master

In a few seconds your files will be published to Azure and your site will be available online.

As we see, **configuration process is very straightforward** and simple. After setting it up it takes** one command to publish** last version of your site to Azure and in a few seconds it will be available online.

* * *

PS: Deploying a website/application using Git is far from being a ground breaking feature as many companies are allowing similar scenarios. Important is who does it – Microsoft. The revamped Azure with all his new features, ongoing involvements in Open Source, the new wave of Developer Tools shows clearly that Microsoft isn’t only serving to “The Enterprise” but also listening carefully to latest tech & software development trends. And even his moves aren’t really innovative, keeping up with new stuff and improving constantly the old products it’s a very good strategy.
