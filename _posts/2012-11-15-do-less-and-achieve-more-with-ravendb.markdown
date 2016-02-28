---
layout: post
title: "Do less and achieve more with RavenDb"
date: 2012-11-15 09:40
comments: true
categories: [ravendb]
---

If you're after a document database for a .NET application it's hard to miss the buzz around [RavenDb](http://ravendb.net). Although there are many other NoSQL databases that are getting much more attention and popularity, RavenDb distinguishes itself by being a native in .NET ecosystem while still offering great value as a generic, technology agnostic data store.

Dealing with Relational Databases, SQL and Object-Relational Mapping are probably the most omnipresent skills in any developer's career. But take a .NET dev with a good background in modern database technologies and let him play few hours with RavenDb and I guarantee that he'll come back impressed.

<!-- more -->

And there is a good reason for that. One of the most impressive things is how easy it to define a class, fill it with data then just throw it in the store. Then load it back an put it on the screen or send over the wires. All this without any object-to-database mappings, no transformations, no base classes; with only few lines of configuration and initialization code. It's fascinating when you're an ORM veteran and experience it for the first time...

I won't go over all of the features of RavenDb. I will just touch few points that, in my opinion, can drastically reduce the amount of infrastructure code of almost any application, be it a client-server "enterprise-grade" system, web site / service or even a rich desktop application.

Disclaimer: YMMV. I'm writing about things I've seen or experienced myself.

## Reduce your mapping layers

Things you probably (or certainly) will not need with RavenDb:

- Maintain a mapping layer to fit your domain model in a relational database with it's rules and constraints
- Keep on the eternal fight with lazy relations
- Map your domain to DTOs and backwards when you're talking over the wires (especially using WCF)
- Map the DTOs to client side models and view models

All this stuff is code. Code to be written, tested and maintained. Take this code and throw it away. Feel better now? I'm glad for you...

Of course you'll have some mapping code. As example you'll probably have to shape your documents to display them on the screen or create static indexes. But I'd consider it as a business/domain concern and not just plumbing code to please your communication layer or mapping framework.  

## Cut through your infrastructure 

What I love in RavenDb is that it's not a mere data store. It comes with a bunch of features and bundles that aren't directly related to persist and query data, but are much more like functional or business components for your application covering real feature requirements. Examples?

#### Search

Any reasonably sized and feature rich application that is manipulating data records is incomplete without an embedded search feature. In .NET world the choice is extremely limited. You'll probably choose the Lucene.NET, write code to sync your search indexes with database/domain changes, then, run scary with the hope to never have to touch that code again. If you've worked with Lucene.NET, you know what I'm talking about...

Much of the RavenDb powers are based on Lucene.NET. But you'll barely observe it hidden behind a friendly API and the "usual suspect", LINQ.

#### Security

You never meet a stakeholder or client obsessed about security? Move on to the next point...

Authorization is hard: Users, Roles, Operations, Permissions. Then add row level security. Oh my, it's really hard. And it's even harder to get it working properly and with minimal penalization on performance. I think because it's a tough challenge there aren't many generic solutions for this problem. 

RavenDb comes with [Authorization Bundle](http://ravendb.net/docs/server/bundles/authorization). The only thing you have to do is to define your rules and (when required) add additional checks for constraints in your business or domain logic.

#### Client notifications

It's called [Changes API](http://ayende.com/blog/157121/awesome-feature-of-the-day-ravendb-changes-api), it let's your client know when a document changes on the server. I've been there, done that: 

- Whenever NHibernate detects a change, send a message about it to a queue. 
- Have a service listening to that queue.
- When there's a message, notify all interested subscribers about it.
- Over WCF. Duplex channel. Silverlight...

And I still have known bugs in that subsystem.

Now take all this stuff and throw it away. Using RavenDb's client API you can subscribe to change events of a single document, a document type or index. An the awesome part - it implements IObservable from [Reactive Extensions](http://www.introtorx.com/). Ditto.

#### Versioning/Audit

With [Versioning Bundle](http://ravendb.net/docs/server/bundles/versioning) RavenDb will keep the history of changes made to a document. With the audit trail you'll be able to find who changed what and when. Depending on your architecture it may be very simple to implement basic audit infrastructure. But it's a much more challenging task to keep around modifications and with relational model is very hard to revert changes when your records were stored in database. With Versioning Bundle it's easy to plug in and know everything.

---

To refrain: all this stuff is code. Code to be written, tested, maintained.

And one more thing. All this components are written by the authors of RavenDb. While I'm confident in my skills, I know my limits. I wouldn't be able to do a better job integrating with a third-party system than the people who wrote it from scratch.

I've highlighted only some of typical parts of a reasonably complex software system but there are more, of course. With RavenDb you already have functional blocks ready for integration and use. This is a point to consider when time-to-marked is critical or you care about your product. And less of a concern when you're only after your monthly paycheck.

Even if you have already some of these subsystems in place and are using "typical" architecture with a service layer, ORM and relational database, you should seriously consider RavenDb while you're at early stages of your product. 

Because with RavenDb you can do less but achieve more by focusing on things that really matter: your domain model, business knowledge and that bizarre wrapper around it, called User Interface.
