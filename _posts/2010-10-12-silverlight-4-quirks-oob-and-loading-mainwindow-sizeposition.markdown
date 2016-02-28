---
comments: true
date: 2010-10-12 15:07:48
layout: post
title: 'Silverlight 4 quirks: OOB and loading MainWindow size/position'
categories: [silverlight]
---

Since we’re writing an Out-Of-Browser Silverlight application, we want it to look and behave like a normal windows app. One of the first things I’m adding to new apps, is saving Window’s position and size and reloading them when application starts next time. So, when user launches the app again, it will find the app same size and position as before

In WPF that’s done pretty straightforward and it just works! For Silverlight 4 it took me already half of the day, and it’s not done yet...

To save MainWindow’s position and size in Silverlight 4 you can attach to MainWindow’s Closing event where you can pick values for current size and position. Then, when loading app next time, in Application’s Startup event we can _try load _previous values.

Where’s the catch? Width and Height are set properly, while setting Left and Top properties has NO EFFECT. Then, if the window is “snapped” to margins (in Windows 7,  maximized for full screen's  height, leaving width the same) it will not be able to restore to previous  height, saying that value is out of bounds.

The code, very simple and straightforward:

    public class MainWindowSettings
    {
        public static void Initialize()
        {
            if(!Application.Current.IsRunningOutOfBrowser)
                return;
            if (!Application.Current.HasElevatedPermissions)
                return;
            Initialize(Application.Current.MainWindow);
        }
        private static void Initialize(Window mainWindow)
        {
            var appSettings = new ApplicationSettings();
            var settings = appSettings.Read<Settings>(Settings.Key, null);
            if (settings != null)
            {
                mainWindow.WindowState = settings.WindowState;
                if (settings.WindowState != WindowState.Maximized)
                {
                    mainWindow.Height = settings.Height;
                    mainWindow.Width = settings.Width;
                    mainWindow.Top = settings.Top;
                    mainWindow.Left = settings.Left;
                }
            }
            mainWindow.Closing += (sender, args) =>
            {
                var windowSettings = new Settings
                {
                    Height = mainWindow.Height,
                    Width = mainWindow.Width,
                    Top =  mainWindow.Top,
                    Left = mainWindow.Left,
                    WindowState = mainWindow.WindowState
                };
                new ApplicationSettings().Write(Settings.Key, windowSettings);
            };
        }
        public class Settings
        {
            public const string Key = "MainWindowSettings";
            public Settings()
            {
                WindowState = WindowState.Normal;
            }
            public double Width { get; set; }
            public double Height { get; set; }
            public double Top { get; set; }
            public double Left { get; set; }
            public WindowState WindowState { get; set; }
        }
    }

Anybody have a clue? An advice, other than “wait Silverlight v. Next”?

**Update**: I've copy/pasted this code in new empty app and discovered that saving Top&Left values is working as it should. But the problem with "snap" is still here. Googling it revealed that [I'm not alone](http://forums.silverlight.net/forums/p/204612/479374.aspx) facing this [issue](http://forums.silverlight.net/forums/p/199966/466463.aspx). So, waiting Silverlight v. Next...
