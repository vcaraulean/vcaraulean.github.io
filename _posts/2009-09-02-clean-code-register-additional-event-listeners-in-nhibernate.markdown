---
comments: true
date: 2009-09-02 08:49:02
layout: post
title: 'Clean code: Register additional event listeners in NHibernate'
categories: [nhibernate]
---

"why is this a problem?" may ask you...

Let's review the ways that [NHibernate](http://nhforge.org/) lets you register your own event listeners. You can do it in configuration file:

    <listener class="NHTest.Listeners.CustomSaveEventListener, NHTest" type="save" />

... or in code:

    Configuration cfg = new Configuration();  
    cfg.EventListeners.SaveEventListeners =   
    new ISaveOrUpdateEventListener[] {new CustomSaveEventListener() };  

Nice, so far...

It's not if you want to **add** a listener, not to replace them. Both registrations are just replacing default event listeners with your own implementations. Very often it's not want you want!

In our case, we want default listeners do the job and just add few our own listeners to run the business. No problem, let's code:
    
    var listeners = configuration.EventListeners;
    listeners.SaveEventListeners = 
        new List<ISaveOrUpdateEventListener>(listeners.SaveEventListeners)
        {new CustomSaveEventListener()}
        .ToArray();

Since I wrote that code some day I hated it. Too verbose, too obscure, hard to read & understand. But event registration infrastructure doesn't offer an easy and simple way to add a listener. Few times I tried to come with a nice solution for that, even tried to modify NH's code. 

Today, I've done it:
    
    configuration.AddListener(el => el.SaveEventListeners, new CustomSaveEventListener());

Some expression's magic and we end up with a very **clean & readable code**.

Implementation:

    public static class NHibernateConfigurationExtensions
    {
        public static void AddListener<TListener>(
            this Configuration configuration, 
            Expression<Func<EventListeners, TListener[]>> expression, 
            TListener listenerImpl)
        {
            var propertyInfo = ReflectionHelper.GetProperty(expression);
            var existentListeners = (TListener[]) propertyInfo.GetValue(configuration.EventListeners, null);
            var newListeners = new List<TListener>(existentListeners) { listenerImpl }.ToArray();
            propertyInfo.SetValue(configuration.EventListeners, newListeners, null);
        }

        public static void AddListeners<TListener>(
            this Configuration configuration, 
            Expression<Func<EventListeners, TListener[]>> expression, 
            IEnumerable<TListener> listeners)
        {
            foreach (var listener in listeners)
            {
                configuration.AddListener(expression, listener);
            }
        }
    }    

Have a nice day...
