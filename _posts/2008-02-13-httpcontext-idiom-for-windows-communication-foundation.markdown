---
comments: true
date: 2008-02-13 14:13:37
layout: post
title: HttpContext idiom for Windows Communication Foundation.
categories: [development, wcf]
---

WCF infrastructure allows you to store context sensitive data in InstanceContext of the service instance. For that you should implement from IExtension<InstanceContext> and plug that class into WCF's infrastructure in any of available ways. 

When I worked on a class that can store contextual information in Web context or WCF context depending on some configuration parameters, I preferred to have similar idioms, and I wrote an HttpContext-like class for WCF.

    ///<summary>
    /// This class incapsulates context information for a service instance
    ///</summary>
    public class WcfInstanceContext : IExtension<InstanceContext>
    {
        private readonly IDictionary items;

        private WcfInstanceContext()
        {
            items = new Hashtable();
        }

        ///<summary>
        /// <see cref="IDictionary"/> stored in current instance context.
        ///</summary>
        public IDictionary Items
        {
            get { return items; }
        }

        ///<summary>
        /// Gets the current instance of <see cref="WcfInstanceContext"/>
        ///</summary>
        public static WcfInstanceContext Current
        {
            get
            {
                WcfInstanceContext context = OperationContext.Current.InstanceContext.Extensions.Find<WcfInstanceContext>();
                if (context == null)
                {
                    context = new WcfInstanceContext();
                    OperationContext.Current.InstanceContext.Extensions.Add(context);
                }
                return context;
            }
        }

        /// <summary>
        /// <see cref="IExtension{T}"/> Attach() method
        /// </summary>
        public void Attach(InstanceContext owner) { }

        /// <summary>
        /// <see cref="IExtension{T}"/> Detach() method
        /// </summary>
        public void Detach(InstanceContext owner) { }
    }

Now, you can use this class to store and retrieve data in the same manner as you're working with HttpContext:
    
    WcfInstanceContext.Current.Items["key"] = new MyClass();
    MyClass myClass = WcfInstanceContext.Current.Items["key"] as MyClass;

Of course, when doing this you should be inside of WCF session...

Enjoy!
