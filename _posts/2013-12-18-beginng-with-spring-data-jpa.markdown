---
layout: post
title: Beginning with Spring Data JPA
description: Beginning with Spring Data JPA, Spring Data JPA example.
comments: true
categories:
- Java
tags:
- Spring
- Java
- JPA
author: sureshsajja
---

In my [previous post](http://coderevisited.com/beginning-jpa-2-0/), I briefly wrote about JPA, a Java specification for accessing, persisting, and managing data between Java objects / classes and a relational database.

In this post, I will talk about Spring Data JPA with a simple example.

## Why Spring Data JPA?

The data access code which uses the Java Persistence API contains a lot of unnecessary boilerplate code.
Too much boilerplate code has to be written  If we have to write dynamic queries or implement pagination.

Spring Data JPA is another layer of abstraction for the support of persistence layer in spring context.
The goal of Spring Data repository abstraction is to significantly reduce the amount of boilerplate code required to implement data access layers for various persistence stores.
We just have to write repository interfaces, including custom finder methods, and Spring will provide the implementation automatically.

## Defining repository interfaces

As a first step we define a domain class-specific repository interface.
The interface must extend Repository and be typed to the domain class and an ID type.
If you want to expose CRUD methods for that domain type, extend CrudRepository instead of Repository.
The CrudRepository provides sophisticated CRUD functionality for the entity class that is being managed.
We can add additional query methods to this repository interface.

```java
public interface EmployeeRepository extends CrudRepository<Employee, Integer> {
}
```

Note: Employee entity class is defined in [previous post](http://coderevisited.com/beginning-jpa-2-0/).

## XML configuration

Each Spring Data module includes repositories element that allows you to simply define a base package that Spring scans for you.
Spring bean configurations are used to configure JPA vendor, data source for creating entity manager.

{% gist b513e8594d9eb4a44e0253299edf89bb %}

## Querying Repository

We can execute query methods from application class by referring to repository bean.

{% gist fdcb7c6180707b7f98385eafb7f94a33 %}


The above application code when executed would insert 3 rows to Employee table in test database.

The code for this example can be downloaded from [here](https://github.com/sureshsajja/SpringDataJPA)
