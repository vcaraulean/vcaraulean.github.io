---
comments: true
date: 2011-09-13 21:43:00
layout: post
title: How to make Caliburn.Micro and Silverlight resources in MergedDictionaries play nicely together
categories: [development, silverlight]
---

I’ve fixed an error in our codebase that prevented Caliburn’s bootstrapper and application resources to live happily together. That was manifesting like warnings in xaml about missing resources and styles, no right styles in Visual Studio designer and so on.

Now, to reproduce the problem we have to follow Caliburn’s guidelines then make few changes:

  * Create new Silverlight 4 application  
  * Add Caliburn.Micro NuGet package  
  * [Configure your application](http://caliburnmicro.codeplex.com/wikipage?title=Nuget) to bootstrap Caliburn  
  * Add new Resource Dictionary to host your common styles/brushes/whatever  
  * Include your dictionary in MergedDictionaries section  
  * Add a fancy color brush and use it in ShellView.xaml by referencing it as a static resource  
  * Build your app

Your application will compile and run fine. However, you can spot few **issues** with your code:

  * ShellView.xaml:  
    * Xaml editor displaying warning: “The resource could not be resolved”  
    * VS Designer will not use your resource on design surface
  * App.xaml  
    * Xaml editor displaying warning: “The designer does not support loading dictionaries that mix 'ResourceDictionary' items without a key and other items in the same collection. Please ensure that the 'Resources' property does not contain 'ResourceDictionary' items without a key, or that the 'ResourceDictionary' item is the only element in the collection.”

To solve this problem you have to modify your App.xaml.

The original App.xaml fragment, as by following recommendations:
    
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Themes/Generic.xaml" />
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
        <local:AppBootstrapper x:Key="bootstrapper" />
    </Application.Resources>

Now, the **corrected version** that removes all issues I’ve described earlier:

    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="Themes/Generic.xaml" />
                <ResourceDictionary>
                    <local:AppBootstrapper x:Key="bootstrapper" />
                </ResourceDictionary>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>

Actually, this fragment is the **recommended way of instantiating boostrapper for WPF** version of Caliburn.Micro. Why it isn’t recommended also for Silverlight? I don’t know. For our rather large project I haven’t seen any downgrades and issues using this approach. Just few annoying bugs were gone...
