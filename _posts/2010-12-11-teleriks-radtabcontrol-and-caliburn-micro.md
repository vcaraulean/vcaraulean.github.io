---
comments: true
date: 2010-12-11 01:00:54
layout: post
title: Telerik's RadTabControl and Caliburn.Micro
categories: [silverlight, tools]
---

**NOTE:** A more complete solution is available in [this blog post](/blog/2011/06/05/integration-of-caliburn-micro-and-teleriks-silverlight-controls/). It contains code, examples and a NuGet package

Well, again a story about having a problem, searching for a solution and finally sharing it :)

So, this time it’s about trying to marry the RadTabControl from Telerik’s control set with Caliburn.Micro. I’m evaluating Telerik for our pilot project in Silverlight since I would like to speed up our development process in few areas. And then we’re using happily Caliburn.Micro in it’s Silverlight flavor.

Scenario: a RadTabControl should contain multiple individual TabItems composed in runtime and backed each by a View Model. And it should be completely ViewModel driven.

Let’s start with ViewModel, as a central piece of our code:

    public class MainPageViewModel : Conductor<IScreen>.Collection.OneActive
    {
        private readonly FirstTabItemViewModel first;
        private readonly SecondTabItemViewModel second;

        public MainPageViewModel(FirstTabItemViewModel first, SecondTabItemViewModel second)
        {
            this.first = first;
            this.second = second;
        }

        protected override void OnInitialize()
        {
            base.OnInitialize();
            Items.Add(first);
            Items.Add(second);

            ActivateItem(first);
        }
    }

Pretty straightforward. 

Now, we have to wrap this model in a view. In better traditions of Caliburn.Micro it’s **extremely succinct and simple**:

    <telerik:RadTabControl x:Name="Items">
        <telerik:RadTabControl.ItemTemplate>
            <DataTemplate>
                <TextBlock Text="{Binding DisplayName}" />
            </DataTemplate>
        </telerik:RadTabControl.ItemTemplate>
    </telerik:RadTabControl>

It looks nice, but it actually doesn’t work. Yet, Caliburn.Micro is handling this way the TabControl from WPF but that of Silverlight is horribly broken (and MS isn’t in hurry to fix it). To have it work nicely with Telerik’s Silverlight RadTabControl we have to add another convention to Caliburn to hint how he should be dealing when doing bindings for RadTabControl. The code is very similar to binding conventions for WPF’s TabControl, I’ve simply adapted it to handle new case.

We will be adding new convention and we should do that in our override of Configure method in our Bootstrapper. The code:

    ConventionManager
        .AddElementConvention<RadTabControl>(RadTabControl.ItemsSourceProperty,
                                                          "ItemsSource",
                                                          "SelectionChanged")
        .ApplyBinding = (viewModelType, path, property, element, convention) =>
        {
            if (!ConventionManager.SetBinding(viewModelType, path, property, element, convention))
                return false;

            var tabControl = (RadTabControl)element;
            if (tabControl.ContentTemplate == null
                && tabControl.ContentTemplateSelector == null
                && property.PropertyType.IsGenericType)
            {
                var itemType = property.PropertyType.GetGenericArguments().First();
                if (!itemType.IsValueType && !typeof(string).IsAssignableFrom(itemType))
                    tabControl.ContentTemplate = ConventionManager.DefaultItemTemplate;
            }
            ConventionManager.ConfigureSelectedItem(element,
                                                    RadTabControl.SelectedItemProperty,
                                                    viewModelType,
                                                    path);

            if (string.IsNullOrEmpty(tabControl.DisplayMemberPath))
                ConventionManager.ApplyHeaderTemplate(tabControl,
                                                      RadTabControl.ItemTemplateProperty,
                                                      viewModelType);
            return true;
        };

And **it works**. 

I’ve posted complete source code with my solution on GitHub, [RadTabControlAndCaliburn](https://github.com/vcaraulean/BlogSamples/tree/master/RadTabControlAndCaliburn).

**Update 14/04/2011**: Updated convention code to work with latest Caliburn.Micro 1.0 RTW.
