---
layout: post
title: Beginning with JPA 2.0
description: Beginning with JPA 2.0, Example for JPA 2.0
comments: true
categories:
- Java
tags:
- Java
- JPA
author: sureshsajja
---

The Java Persistence Architecture API (JPA) is a Java specification for accessing, persisting, and managing data between Java objects / classes and a relational database.
JPA allows POJO (Plain Old Java Objects) to be easily persisted.
JPA allows an object’s object-relational mappings to be defined through standard annotations or XML defining how the Java class maps to a relational database table.
JPA also defines a runtime EntityManager API for processing queries and transaction on the objects against the database.
JPA defines an object-level query language, JPQL, to allow querying of the objects from the database.

## Why JPA

It is a standard for the management of persistence and object/relational mapping with Java EE and Java SE.
JPA supports the large data sets, data consistency, concurrent use, and query capabilities of JDBC.
Like object-relational software and object databases, JPA allows the use of advanced object-oriented concepts such as inheritance.
JPA avoids vendor lock-in by relying on a strict specification. JPA focuses on relational databases. And JPA is extremely easy to use.
Currently most of the persistence vendors have released implementations of JPA confirming its adoption by the industry and users.
These include Implementations include Hibernate, TopLink, and Kodo JDO, Cocobase and JPOX.
Java Persistence consists of four areas
	
* The Java Persistence API
* The query language
* The Java Persistence Criteria API
* Object/relational mapping metadata

## Entities

In Object oriented paradigm, An entity is a lightweight persistence domain object.
Typically, an entity represents a table in a relational database, and each entity instance corresponds to a row in that table.
Changing values of entity instance works just like changing values on any other class instance.
The difference is that you can persist those changes in database.
The primary programming artifact of an entity is the entity class, although entities can use helper classes.
The persistent state of an entity is represented through either persistent fields or persistent properties.
These fields or properties use object/relational mapping annotations to map the entities and entity relationships to the relational data in the underlying data store.

## Characteristics of an Entity
	
1. Persistability
* An Entity is persistable since it can be saved to a persistence store.
But this doesn’t happen automatically. It has to be invoked by API.
It is important because it leaves control over persistence to application.

2. Identity
* Identifier, is the key that uniquely identifies an entity instance and distinguishes it from all the other instances of the same entity type.

3. Transactionality
* Changes made to the database either succeed or fail atomically, so the persistent view of an entity should indeed be transactional.

4. Granularity
* Entities are meant to be fine-grained objects and they are business domain objects that have specific meaning to the application that accesses them.

## Example Entity class with annotations metadata

{% gist 00414eb94408f4a83c9b46abaadc14ec %}

## Managing Entities

Entities are managed by the entity manager.
Each EntityManager instance is  associated with a persistence context: a set of managed entity instances that exist in a particular data store. A persistence context defines the scope under which particular entity instances are created, persisted, and removed.
The EntityManager interface defines the methods that are used to interact with the persistence context.

EntityManager is to be implemented by a particular persistence provider.
It is the provider that supplies the backing implementation engine for the entire Java Persistence API, from the EntityManager through to implementation of the query classes and SQL generation.

## Illustration of Entity manager

{% gist ccf0cb74f37d4e08c9e207e0e98cc31f %}

## Project setup

* Create a  simple java project with maven [http://maven.apache.org/plugins-archives/maven-archetype-plugin-1.0-alpha-7/examples/simple.html](http://maven.apache.org/plugins-archives/maven-archetype-plugin-1.0-alpha-7/examples/simple.html)
* Maven dependencies

{% gist 13ed590a339fbb0ab0afa06d13c1fbce %}

* Setup mysql and use the following sql code to create "Employee" table in test database

{% gist 9b693418897c37e406faa874e4bce54a %}
	
* Create META-INF/persistence.xml file in resources directory. It specifies JPA provider and it's properties.

{% gist b43a5a53ece15137cba18d58049f1b6c %}

* Add Employee.java, EmployeeTest.java files to project
* You should be able to execute EmployeeTest which will insert 3 records to Employee table.


The sample code can be downloaded from [here](https://github.com/sureshsajja/JPA101)
