---
comments: true
date: 2008-12-18 17:33:17
layout: post
title: 'NHibernate''s niceties: hbm2ddl auto mode in unit tests'
categories: [nhibernate, testing]
---

Very often you're going over a description of some function or new feature and just notice in your mind, "nice feature" without seeing a real application for it. Later, you're coming with scenario where the trick you've read days ago will fit perfectly.

That was the case when I've seen earlier in [Tuna Toksoz](http://www.tunatoksoz.com/)'s weblog a [description of "hbm2ddl.auto" property](http://www.tunatoksoz.com/post/NHibernate-hbm2ddl.aspx). Few days ago, I've put it to work in a way that makes me feel better. By the way, the particular reason that made me happy was that I've deleted a lot of repetitive code from our code base.

#### How you can use hbm2ddl.auto property in unit tests

Our integration tests are run over a real database. For each test we're re-creating the database, so that each test will be ran in separation and without interfering with results of previous tests. Also, things are a bit complicated since we're working with few databases at the same time, each with his own schema. Before, each test had to use SchemaExport class that exported the database schema built from NHibernate configuration on the way deleting your old database/schema/data. So, each integration test class had same method to call. Redundant and repetitive. Yes, you can make a base class that will do that, but you have to inherit then all tests from that class.

**How it works now:** in session configuration (usually in app.config for integration test project) you put:

    <property name="hbm2ddl.auto">create</property>

... and you're set. Each time your code will build a new SessionFactory, NHibernate will call:

    new SchemaExport(cfg).Create(false, true);

This way, before the SessionFactory will be built, your database will be refreshed with a new schema and tests will run in a clean database.

One line of code instead of tens or hundreds. Isn't that a good trick?!
