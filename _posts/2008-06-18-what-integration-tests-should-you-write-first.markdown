---
comments: true
date: 2008-06-18 10:18:10
layout: post
title: What integration tests should you write first
categories: [castle, nhibernate, testing]
---

Generally, the goal of integration tests is to verify that different application parts are glued well together and interacts as expected. Thus, they are a bit different thing from unit tests that tends to test how a small, separate part is acting. Below are two tests that I can't classify to be pure integration tests because they touch a sensible part of application/infrastructure configuration like Windsor's configuration files and NHibernate's mappings. These simple tests can save a lot of time at very initial development stage when application bits are changed frequently and later, as a guarantee that you've not broken something in a recent refactoring.

#### Can_resolve_all_services_in_container()

[Bill Simser](http://weblogs.asp.net/bsimser/default.aspx) wrote some time ago about [the first test to be written when using Castle's Windsor Container](http://weblogs.asp.net/bsimser/archive/2008/06/04/the-first-spec-you-should-write-when-using-castle.aspx). It's a real gem that let's you verify if any of services registered in container can be resolved (have all required dependencies). Here is it, a bit improved for skipping verification for generic types, since, at that moment, we don't know the generic argument for type to be constructed:
    
    [Test]
    public void Can_resolve_all_services_in_container()
    {
        var container = new WindsorContainer("container.xml");
        foreach (IHandler handler in container.Kernel.GetAssignableHandlers(typeof(object)))
        {
            if (handler.ComponentModel.Service.IsGenericType)
                continue;

            container.Resolve(handler.ComponentModel.Service);
        }
    }

#### Can_compile_NHibernate_mapping_configuration()




This test was sitting for a long time in our codebase, but I've seen it recently in [this post](http://codebetter.com/blogs/david_laribee/archive/2008/06/17/test-your-nhibernate-mappings.aspx) of [David Larebee](http://codebetter.com/blogs/david_laribee/default.aspx). I wrote it in times when writing our first mappings and the only one way to verify if you don't have any faults was to write one of CRUD operations and run the test. But that's a long process. Shorter response cycle is always better. Here it is:
    
    [Test]
    public void Can_compile_NHibernate_mapping_configuration()
    {
        ISessionFactory factory =
            new Configuration()
                .Configure()
                .AddAssembly("MyApp.MyDomainAssembly")
                .BuildSessionFactory();

        Assert.IsNotNull(factory);
    }

This test suppose that you have HNibernate configuration in your app.config and tries to register all mappings from given assembly file.
