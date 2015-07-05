---
title: Hibernate vs MyBatis
tags: [java, ORM, hibernate, myBatis]
---

###Comparison of MyBatis versus Hibernate
I’ve often heard people arguing about whether Hibernate is better or MyBatis. The truth is, “there is no spoon.” It is not about whether Hibernate is better or MyBatis, but which of these tools best fits your situation. Without further ado, here is a comparison.

_*Disclaimer: This is my personal (and humble) opinion. Some databases were irremediably harmed in the forming of these views.*_

##MyBatis
It is primarily data mapper with a database-centric view of things. You map result sets to objects without giving a rat’s rear-end about whether that data came from single/multiple tables, stored procs etc.

###Pros:
+ simplicity and faster development
+ good for writing pure sql (stored procs, functions included, complex queries)

###Cons:
- SQLs might be tied to a specific database vendor
- No in-built caching, paging mechanism (querying with max records actually does a full table scan)

###When to Use:
* use when complete control over SQL is needed, especially if you have good knowledge of SQL.
* data model doesn’t cleanly match object model or can change/get complex over time.

##Hibernate
This is primarily an Object-Relational Mapper with an object-centric view of the world. In other words, it maps your object model to database tables with relatively little effort from your side (if the object model is in sync with data model).

###Pros:
+ database-agnostic query language
+ automatic SQL generation, OOPS concepts supported
+ HQL-level object caching, in-built pagination => high scalability.

###Cons:
- fine tuning usually required and sessions are a pain to get right
- schema changes can cause problems
- complexity cost

###When To Use:
* use when data-object mapping is synced and you have complete control over db.
* when writing SQL is painful or complex.