---
layout: post
title: "Book review - NoSQL Distilled"
date: 2012-12-14 09:38
comments: true
categories: [books, nosql]
---

[NoSQL Distilled: A Brief Guide to the Emerging World of Polyglot Persistence](http://www.amazon.com/NoSQL-Distilled-Emerging-Persistence-ebook/dp/B0090J3SYW) by Martin Fowler and Pramod J. Sadalage.

For some time already I've been keeping an eye on rising popularity of NoSQL. From reading articles and blog posts about various aspects and flavors, I adventured myself and dived more deeply into RavenDb, a NoSQL Document Database. Still, the picture I had in my mind was somewhat blurry and had few areas where I wasn't sure that I understand some basic things.
<!-- more -->
This book is a very good starting point into NoSQL world. It starts with explaining the rise of popularity of NoSQL solutions for multiple domains and modern data storage requirements. Then it explains important concepts around NoSQL data stores like aggregation and distribution models, consistency, versioning and Map-Reduce. The second part of the book takes a look at each family of NoSQL flavors and analyzes their features, data modeling challenges and applicability of concrete database in various domains. Key-Value databases are presented by [Riak](http://basho.com/products/riak-overview/), Document Databases by [MongoDB](http://www.mongodb.org/) with few mentions of [RavenDb](http://ravendb.net/), [Cassandra](http://cassandra.apache.org/) represents the Column-Family store type, and, final group, the Graph databases are explained using [Neo4J](http://www.neo4j.org/). Last chapters of the book talk about schema migrations, data storage options outside of NoSQL area and what you should consider when choosing a database.

One of the ideas frequently discussed trough the whole book is why people are choosing NoSQL solutions: 

> There are two primary reasons for considering NoSQL. One is to handle data access with sizes and performance that demand a cluster; the other is to improve the productivity of application development by using a more convenient data interaction style.

Most often when you speak about NoSQL with somebody the reaction looks like "ah, that thing that Google and Amazon runs on thousands of servers". And nobody who only have heard of NoSQL is sensing the second great advantage of these systems: improving productivity and flexibility of your development team and make the life of application developer easier.

NoSQL doesn't means a complete schema-less data storage. Instead of having rigid definitions of schema and relationships as in the world of Relational Database, the shape of data is defined by application itself. That is, application is defining how it wants to store and access the data. No more you have to fight the impedance mismatch between relational model and in-memory data structures. Forget the performance penalization of an ORM, limitations of relational model in querying, aggregating and shaping data. Instead, your application defines the aggregate model and naturally persist it. Then it has reach options to slice and dice stored data, perform flexible Map-Reduce transformations and offers reach analytical capabilities. This allows development teams to be more flexible and productive when building complex systems. When your data storage is a simple commodity, easy to access and manipulate, you will have much more time to work on your domain, business logic or User Interface.

Each kind of NoSQL implementation offers it's own advantages and has it's weak points. For me the most interesting are the Document Databases like MongoDB and RavenDb. They will suit a very broad range of applications because of the notion of a Document - something with properties and values aggregated in one entity stored as a simple JSON document. This will give you ability to index documents, write rich document based queries and much more.

### Summary

A great overview of existent NoSQL technologies, their characteristics and applicability of each of them in various domains. The book is short as it was really meant as an overview, not a deep dive in these technologies. 

At the moment when I write this review Amazon places this book as #1 in Databases category and #2 in Databases/SQL, it has 15 reviews with an overall score of 4.5. 

Highly recommended.