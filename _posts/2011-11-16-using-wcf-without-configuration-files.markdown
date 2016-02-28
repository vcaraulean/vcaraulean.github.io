---
comments: true
date: 2011-11-16 00:14:59
layout: post
title: Using WCF without configuration files
categories: [development, wcf]
---

I have recently published new project on GitHub, [WcfWithoutConfigFile](http://github.com/vcaraulean/WcfWithoutConfigFile). This post is intended as a ‘readme’ file for this repository.

It’s just a collection of simple projects that I’ve put together when a coworker asked me to show how he can use WCF without configuration files. He got feed up working with XML in web.config and wanted a cleaner and more understandable way to configure Windows Communication Foundation.

What’s in:

  * How to use WCF in your unit tests ([WcfWithoutConfigFile.Tests](http://github.com/vcaraulean/WcfWithoutConfigFile/tree/master/WcfWithoutConfigFile.Tests) project). Sometimes you want to get your code tested at service boundaries and you want to involve also WCF. It contains, among a simple unit test, a base class for your tests so the test cases are clean and don’t contain plumbing code.  
  * Hosting WCF services in IIS ([WcfWithoutConfigFile.WebHost](http://github.com/vcaraulean/WcfWithoutConfigFile/tree/master/WcfWithoutConfigFile.WebHost) project). Defining the service in .svc files and all required infrastructure to instantiate the service via code-only configuration.  
  * **My favorite** – hosting WCF service in IIS using Castle’s [WcfIntegration facility](http://www.castleproject.org/container/facilities/trunk/wcf/index.html) ([WcfWithoutConfigFile.WebHost.Castle](https://github.com/vcaraulean/WcfWithoutConfigFile/tree/master/WcfWithoutConfigFile.WebHost.Castle) project). It leverages Castle Windsor container and WcfFacility to easily host/run/consume your WCF services. It also configures the service to use Windows authentication and shows how to retrieve client’s WindowsIdentity in the service. There is also a client project, [WcfWithoutConfigFile.WebHost.Castle-Client](https://github.com/vcaraulean/WcfWithoutConfigFile/tree/master/WcfWithoutConfigFile.WebHost.Castle-Client) that connects to service and does some operations.

[WcfWithoutConfigFile.WebHost.Castle](https://github.com/vcaraulean/WcfWithoutConfigFile/tree/master/WcfWithoutConfigFile.WebHost.Castle) requires some configuration to get it working properly. Be sure to:

  * Run it in IIS Express  
  * Enable Windows Authentication in project properties
