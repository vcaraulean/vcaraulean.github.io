---
comments: true
date: 2011-12-09 00:45:10
layout: post
title: Using WP7's ProgressIndicator with Caliburn.Micro
categories: [silverlight, windows-phone, caliburn-micro]
---

The Windows Phone SDK 7.1 introduced a new control to show the progress of long operations or interactions, ProgressIndicator. As I love much the [Caliburn.Micro](http://caliburnmicro.codeplex.com) framework I got some time to integrate neatly ProgressIndicator with the rest of application and services using Caliburn’s container.

First of all, let’s define our application facing progress service:

    public interface IProgressService {
        void Show();
        void Show(string text);
        void Hide();
    }

Pretty simple, yes...

The implementation was partly inspired by [Jeff Wilcox’s article about creating a global ProgressIndicator](http://www.jeff.wilcox.name/2011/07/creating-a-global-progressindicator-experience-using-the-windows-phone-7-1-sdk-beta-2/).

It’s pretty simple and straightforward: we’re getting the Root Application Frame and hooking up to the Navigated event. His arguments contain the page being navigated to. Then we’re attaching our instance of ProgressIndicator to that page. And the service uses Show/Hide methods to manipulate the state of indicator. The code:

    public class ProgressService : IProgressService {
        const string DefaultIndicatorText = "Loading...";
        private readonly ProgressIndicator progressIndicator;

        public ProgressService(PhoneApplicationFrame rootFrame)
        {
            progressIndicator = new ProgressIndicator {Text = DefaultIndicatorText};

            rootFrame.Navigated += RootFrameOnNavigated;
        }

        private void RootFrameOnNavigated(object sender, NavigationEventArgs args)
        {
            var content = args.Content;
            var page = content as PhoneApplicationPage;
            if (page == null)
                return;

            page.SetValue(SystemTray.ProgressIndicatorProperty, progressIndicator);
        }

        public void Show()
        {
            Show(DefaultIndicatorText);
        }

        public void Show(string text)
        {
            progressIndicator.Text = text;
            progressIndicator.IsIndeterminate = true;
            progressIndicator.IsVisible = true;
        }

        public void Hide()
        {
            progressIndicator.IsIndeterminate = false;
            progressIndicator.IsVisible = false;
        }
    }

The last piece of this mini-puzzle is wiring up the service to container and made it available to the rest of application:

    public class AppBootstrapper : PhoneBootstrapper {
        private PhoneContainer container;

        protected override void Configure()
        {
            container = new PhoneContainer(RootFrame);
            container.Instance<IProgressService>(new ProgressService(RootFrame));


That’s all. Now declare a dependency on IProgressService whenever you need it and show/hide to give a nice feedback to the User when your application is doing something long. You never forget that User is the ultimate chief and judge in our business, right?..
