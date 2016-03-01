---
comments: true
date: 2010-08-05 14:00:33
layout: post
title: Interesting pattern to decouple application parts and for integrating with other apps
---

Udi Dahan has written about an [interesting way to evolve loosely-coupled application](http://www.udidahan.com/2010/07/14/evolving-loosely-coupled-frameworks-and-apps/). It’s a very interesting trick and we’re already using it in our applications. And we’re using it for a very similar scenario: allowing to application modules and infrastructure to step in and say what they have about configuration and bootstrapping.

For example, some modules can modify configuration of [NHibernate](http://nhforge.org/)’s SessionFactory. We have next interface:

	public interface IConfigurationContributor     
	{      
		void Process(string name, Configuration config);      
	} 

And whoever wants to intervene in configuration of any SessionFactory is implementing it. When factory is build container is pulling all implementors of IConfigurationContributor and “runs” over all of them.

As a result - clean design and loosely coupled application parts...
