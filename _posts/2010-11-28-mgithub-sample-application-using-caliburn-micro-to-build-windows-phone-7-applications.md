---
comments: true
date: 2010-11-28 22:37:11
layout: post
title: mGitHub Sample Application. Using Caliburn.Micro to build Windows Phone 7 applications
categories: [community, development, silverlight, windows-phone]
---

#### [mGitHub.SampleApp on GitHub](https://github.com/vcaraulean/mGitHub.SampleApp)

#### [mGitHub - the real app - is available in Marketplace](http://gitstation.com)

#### The story

I’ve been very excited when Microsoft announced a new & shiny mobile platform, the Windows Phone 7. Mostly because it gave me the chance to explore new things using very familiar tools: Visual Studio 2010, C# and Silverlight. I never had enough time to start digging in this area. When I’ve read that Rob Eisenberg announced a [Caliburn.Micro contest](http://devlicio.us/blogs/rob_eisenberg/archive/2010/11/02/caliburn-micro-contest.aspx) and having lost the passion for doing my job at company I’m working now, I’ve decided to participate.

I’ve decided to go with an interesting idea - writing a WP7 app to work with [GitHub](http://github.com/). Mainly because I’m sure that I will use such app myself. GitHub is an awesome service that lets you host and collaborate on your projects freely once they are open source or if you want private projects you can have it too for a fee. I like idea and this sample application so much, the whole development process went so smooth and easy so that I’ve decided, along with participation in CM contest and publishing sources of my sample app on GitHub, to continue developing it by adding more features to offer a complete and user friendly application to work with GitHub (later about this).

#### The app

**mGitHub.SampleApp** allows to browse few preselected projects and repositories on GitHub. You can see user/repository details, navigate trough user’s own repositories, watched repositories, view contributors and so on...

The use of Caliburn.Micro, which is (not only) a MVVM framework, implies that application is View Model driven. Main screen is baked by a view model, then everything else - application flow, user interactions, service calls are done within View Model classes (with few small inclusions of WP7 navigation model, view based).

WP7 controls like **Panorama **and **Pivot** were also used with MVVM in mind. For example, here is the code for main View Model that is serving a Panorama-based view:

    public class MainPageViewModel : Screen {
        public MainPageViewModel(MostViewedViewModel mostViewed,
                                 FavoritesViewModel favorites,
                                 AboutViewModel about)
        {
            MostViewed = mostViewed;
            Favorites = favorites;
            About = about;
        }

        public FavoritesViewModel Favorites { get; protected set; }
        public MostViewedViewModel MostViewed { get; protected set; }
        public AboutViewModel About { get; protected set; }
    }

So, here we’re just injecting our View Models for individual Panorama pages in MainPageViewModel and then [SimpleContainer](http://caliburnmicro.codeplex.com/wikipage?title=SimpleContainer&referringTitle=Documentation) takes care to provide required instances. Since in a Panorama control all pages are visible (active), we’re using a simple Screen (from CM) as our base class for our VM. Then, this is how the panorama view is defined:

    <controls:Panorama Title="mGitHub sample app "> 
        <controls:Panorama.Background> 
            <ImageBrush ImageSource="PanoramaBackground.jpg"/> 
        </controls:Panorama.Background> 
        <controls:PanoramaItem x:Name="Favorites" Header="my favorites" /> 
        <controls:PanoramaItem x:Name="MostViewed" Header="interesting" /> 
        <controls:PanoramaItem x:Name="About" Header="about" /> 
    </controls:Panorama>

It’s nice and clean, with very good separation of concerns: each panorama page has it’s own View and a corresponding View Model. The work to glue it together is done by Caliburn.

The Pivot is a bit different. This control is basically a well known TabControl, it has headers and only one item is active at a time. For a PivotViewModel we’ll use a Conductor class with one active item:

    [SurviveTombstone]
    public class PivotViewModel : Conductor<IScreen>.Collection.OneActive {
    }

Then the two application Pivots, UserPivotViewModel and RepositoryPivotViewModel will just ask from container the view models of individual pages and add them to Items collection of Conductor class:
    
    [SurviveTombstone]
    public class UserPivotViewModel : PivotViewModel {
        private readonly UserDetailsViewModel details;
        private readonly UserRepositoriesViewModel repositoriesViewModel;
        private readonly UserWatchingViewModel watchingViewModel;

        public UserPivotViewModel(UserDetailsViewModel details,
                                  UserRepositoriesViewModel repositoriesViewModel,
                                  UserWatchingViewModel watchingViewModel)
        {
            this.details = details;
            this.repositoriesViewModel = repositoriesViewModel;
            this.watchingViewModel = watchingViewModel;
        }
        protected override void OnInitialize()
        {
            base.OnInitialize();

            Items.Add(details);
            Items.Add(repositoriesViewModel);
            Items.Add(watchingViewModel);

            ActivateItem(details);
        }

The view, again, is very simple, grace of Caliburn.Micro magic:

    <controls:Pivot x:Name="Items" Title="{Binding PivotTitle}" SelectedItem="{Binding ActiveItem, Mode=TwoWay}"> 
        <controls:Pivot.HeaderTemplate> 
            <DataTemplate> 
                <TextBlock Text="{Binding DisplayName}" /> 
            </DataTemplate> 
        </controls:Pivot.HeaderTemplate> 
    </controls:Pivot>

That’s why I enjoy using this framework, it makes some development tasks extremely simple.

Also, application make use of INavigationService,  wrapper API for Launchers/Choosers and is aware of Thombstoning. I tried to keepcode small enough to be easily followed and read, so it will not be too hard to find those places in project’s source code…

Another (probably interesting) piece of code in this project is communication with GitHub using their API. It’s not exactly the IGitHubHost service that I want to highlight, it’s pretty trivial. Take a look at two wrappers around RequestProcessor class, both added in order to improve User Experience when working with mGitHub.SampleApp on the phone:

  * ProgressAwareRequestProcessor – will show a view with [PerformanceProgressBar](http://www.jeff.wilcox.name/2010/11/smooth-loading-performanceprogressbar/) during **any** remote call
  * CachingRequestProcessor – providing a extremely simplistic **data cache**, storing requests and their results

These wrappers are registered in container and View Models even don’t know about their abilities, for them it’s just issuing a call and displaying a result. Separation of concerns is always good principle to follow...

#### Screenshots

Few screenshots, to get the sense of how UI looks like:

![alt mgithub](/../../../../../images/2010/mgithub-1.png)
![alt mgithub](/../../../../../images/2010/mgithub-2.png)
![alt mgithub](/../../../../../images/2010/mgithub-3.png)
![alt mgithub](/../../../../../images/2010/mgithub-4.png)

#### Final notes

This application surely contains bugs, it probably has few strange design decisions and graphics are done by professional developer (me). But **it works**. It serves it’s main purpose to show how you can write an application for Windows Phone 7 using Caliburn.Micro. And I had a lot of fun working on it...

I’ll surely improve this sample, fix reported bugs and will listen carefully to improvement ideas. So, **any** **feedback is welcome**.

But in the meantime, I’m doing everything possible to release a real application to Marketplace that will be called **mGitHub** and will offer essentially **improved experience and more features**. So, if you like the idea, if you like GitHub and will most likely use the app on your phone, I would be glad to hear from you. Next time when I’ll be planning features your feedback will be considered.

This application weren’t tested on a real phone. I’m counting the hours until my hands will get the ordered Samsung Omnia 7 and I’ll be testing this app on real hardware. If everything goes well, expect yearly next week a feedback (and probably fixes & improvements) after using the app on WP7 device.

Code of this application is licensed under MIT license and [available on GitHub](https://github.com/vcaraulean/mGitHub.SampleApp).

#### Credits

Thanks to:

  * Rob Eisenberg and Christopher Bennage for [Caliburn.Micro](http://caliburnmicro.codeplex.com/)
  * [Jeff Wilcox](http://www.jeff.wilcox.name/) for PerformanceProgressBar
  * [Panorama background image](http://www.sxc.hu/photo/1271413) by cobrasoft

Special thanks to:

  * my friend Dumitru Cantemir for helping me with some code.
  * my daughter, for letting her dad working on this app. Even if sometimes it was...

![alt dad at work](/../../../../../images/2010/dadatwork.jpg)
