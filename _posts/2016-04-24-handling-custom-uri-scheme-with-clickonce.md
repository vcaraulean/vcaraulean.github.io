---
layout: post
title: "Handling custom URI Schemes with ClickOnce"
date: 2016-04-24 15:00
comments: true
categories: clickonce, .net
---

*This article will show how to handle URI schemes containing parameters in a ClickOnce applications. For example, a link like `theapp://notificatons?eventId=124` can be handled by a ClickOnce application, where query parameters are passed in and can be handled by application itself. It's also sometimes called `deep link` activation in a ClickOnce context.*

URI scheme is a useful concept. It allows you to register an [URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier) with your OS and then whenever an URL that corresponds to that URI schema will be clicked, you application will be launched. It's very handy in some cases. Handling URI Schemes is [well documented](https://msdn.microsoft.com/en-us/library/aa767914.aspx) and, actually, quite simple. But it gets more though if you want to do it in a ClickOnce application in the case if you want to react to whatever is coming after URI scheme.

In our cases we wanted to send out emails with some specially formatted links (something like `theapp://notificatons?eventId=124`). When clicked, these links should open our application and then navigate to a "notifications" view and load details of a specific event. All this from a **ClickOnce** application. The hard part here is handling the URI arguments, thus the parts `navigation` and `eventId=124` have to be available to the application.

We came with a way to do it and here's a description how it can be done. And there are two ways to do it:
 
 - doing it properly: get rid of ClickOnce. If you can ditch ClickOnce - DO IT. Seriously. 
 - the hard way: read on...
 
## Implementation constraints, details & challenges:

 - ClickOnce application
 - Custom URI scheme
 - Application should be able to handle parameters encoded in URI like this: `theapp://notificatons?eventId=124`
 - Clicking on a such link should launch the application and allow the app to handle launch parameters
 - (constraint) The process should work even if Chrome is set as default browser 
 - (limitation) Activation URI is a web address (approach is not handling apps installed from local or network paths)
 - (limitation) You have to launch application at least once manually to make it work 

## Show me the code!

You can jump straight into the code. I have a WPF project that I used to work on this feature and test activation. [Code is on GitHub](https://github.com/vcaraulean/ClickOnceCustomUriScheme). Most interesting classes are [ApplicationUriSchema.cs](https://github.com/vcaraulean/ClickOnceCustomUriScheme/blob/master/ClickOnceCustomUriScheme/ApplicationUri/ApplicationUriSchema.cs) where all interesting things are happening and you might check also [MainWindowViewModel.cs](https://github.com/vcaraulean/ClickOnceCustomUriScheme/blob/master/ClickOnceCustomUriScheme/MainWindowViewModel.cs) to see how to get the lauch URI and query string parameters. Application is logging all useful info and decisions, so watch the logs to understand what's going on.

## So, how this works?

The trick is to register as a command handler for our URI scheme the location of ClickOnce application. Then, when application is launched from via URI scheme, detect that and start new instance of ClickOnce application with parameters from the initial activation URL and shut down the current application instance. Parameters then are accessible in ClickOnce context via ActivationUri. We got 3 steps here.

#### 1. Register application URI scheme

When application is launched for the first time it will update the Windows Registry to register URI with the application. The shell command that will handle the URI is set to the location of where ClickOnce executable is installed. That's an unique location that changes every time a new version is installed. So every time a new application version is run for the first time, the registry settings that point URI protocol to command location are updated with new .exe file location

#### 2. Restart application launched from URI handler with ClickOnce context
When application is launched from a link with corresponding URI schema two things are happening:

  - starting `iexplore.exe` with parameters list containing the URL to the application's ActivationUrl and a query string containing parameters encoded in where a new process is started having as a launch parameters  
  - shutting down current instance
  
#### 3. Handling startup parameters from ClickOnce context
Now we're running in ClickOnce context with ActivationUrl containing the ClickOnce location and a query string containing URL parameters from original link.

## To sum up

Have a look at the code. But. It's a hack and work-around. It's not 100% bullet-proof and not pretty for users to see (ClickOnce launch dialog is shown then closed to leave place to new instance of application). But it worked for us, in our environment and user base.

But again, if possible, don't tacle such things with ClickOnce. Step up and use a proper install/update/uninstall framework. [WiX](http://wixtoolset.org/) or [Squirrel](https://github.com/Squirrel/Squirrel.Windows) are just few alternatives...
