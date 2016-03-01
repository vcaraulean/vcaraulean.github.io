---
comments: true
date: 2009-04-28 20:27:49
layout: post
title: Speeding up a local build
categories: [development]
---

Jimmy Bogard has a [very nice summary](http://www.lostechies.com/blogs/jimmy_bogard/archive/2009/04/15/speeding-up-a-local-build.aspx) of steps you can do to speed up building of your project or solution. By “build” I mean not just rebuilding solution from scratch, but doing also a complete run down: cleanup, rebuild and run your tests. This can be a local build performed on a developer’s machine, or a continuous integration build performed by dedicated build/integration server.

Some time ago, when build times grew to intolerable limits, we cut down number of projects in our solution. Along with a common output folders, this improved dramatically performance especially on integration server, where a single committed change threw running of few cascading builds and code analyses.

Now, slowest part of our build process are integration tests. A big part of them are “testing integrations” with database. Since we’re using real MS SQL Express 2008 instances to run them, tests are very slow. A natural solution is using of an in-memory database like SQLLite. We spent some time on this, but since our database interactions are not trivial (kind of multi-tenant & multi-database application) we gave up until better times. But I think we will be back to finish that, because with more tests written we’re pushing up build times. To give an idea how this can be done, Ayende Rahien has a very basic example [how to use in-memory database to run NHibernate integration tests](http://ayende.com/Blog/archive/2009/04/28/nhibernate-unit-testing.aspx).
