---
comments: true
date: 2009-07-15 16:04:12
layout: post
title: WPF niceties. Collection grouping in ViewModel
categories: [wpf]
---

Having to group a collection shown in UI by a command fired from the View, I've came to a nice solution by writing an extension method to the `ObservableCollection<T>` class.

The extension method:
    
    public static class ObservableCollectionExtensions
    {
        public static void VisualGroupingBy<T>(this ObservableCollection<T> collection,
                                               Expression<Func<T, object>> groupByExpresson)
        {
            var view = CollectionViewSource.GetDefaultView(collection);
            collection.VisualGroupingClear();

            var propertyName = ReflectionHelper.GetPropertyName(groupByExpresson);

            view.GroupDescriptions.Add(new PropertyGroupDescription(propertyName));
        }

        public static void VisualGroupingClear<T>(this ObservableCollection<T> collection)
        {
            var view = CollectionViewSource.GetDefaultView(collection);
            view.GroupDescriptions.Clear();
        }
    }

How to use it:
    
    public ObservableCollection<Address> Addresses { get; set; }
    public void GroupAddressesByCity()
    {
        Addresses.VisualGroupingBy(address => address.City);
    }

Clean, no magic strings, readable...
