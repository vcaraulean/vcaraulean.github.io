---
comments: true
date: 2011-12-13 00:04:22
layout: post
title: Using WP7's Secondary Tiles with Caliburn.Micro
categories: [windows-phone, caliburn-micro]
---

There’s good guide on [how to manage WP7’s Secondary tiles](http://msdn.microsoft.com/en-us/library/hh202979(v=vs.92).aspx) on MSDN.

So, how are you doing it with Caliburn.Micro? Take same way, it just works!

However, there’s a small touch of Caliburn.Micro goodness you can add to your code to make it more maintainable. When you’re setting the URI for your Tile, use the INavigationService.UriFor<>() extension method. This will make your code a little more friendly for an eventual refactoring and for sure will save you from few nasty bugs.

Sample code:
    
    var newTileData = new StandardTileData
    {
        Title = "my tile",
        BackTitle = "my tile details",
        BackContent = "description",
    };

    ShellTile.Create(navigationService.UriFor<MyViewModel>()
                         .WithParam(x => x.ViewModelParameter, "some value")
                         .BuildUri(),
                     newTileData);

Piece of cake!
