---
author: sureshsajja
comments: true
date: 2013-02-15 04:55:07+00:00
layout: post
link: http://coderevisited.com/singleton/
slug: singleton
title: Singleton
wordpress_id: 364
categories:
- Interview
- Java
---

## What is singleton?


A singleton is simply a class that is instantiated exactly once



## Objective of singleton?


There should be only one instance allowed for a class
There should allow global point of access to that single instance



## Why singletons are required?


Singletons typically represent a system component that is intrinsically unique, such as Java Runtime, the window manager or file system
Singletons are used to store data that it is used/updated across multiple components. The data in one component is usually important to another component, so everything is managed in one central object.



## Ways to implement


There are 2 ways to implement singletons. In Both ways, We suppress the constructor and don’t allow even a single instance for the class. But we declare a static member to hold the sole instance of our singleton class



### Early Initialization 


At the time of loading the class, instance gets created. JVM guarantees that the instance will be created before any thread access the static member. getInstance() method simply returns the instance that was already created.
If your application always creates and uses singleton instance or the overhead of creation and runtime aspects of the singleton are not onerous, this early initialization is preferred.

 
 








### Lazy Initialization 


 
 




By Creating a static factory method, sole instance gets created. The getInstance() method gives us a way to instantiate the class and also to return an instance of it.
In this scenario, we need special attention when multiple threads are accessing getInstance() method.



#### Synchronizing getInstance() Method


 





It works perfectly fine in multithreaded scenario. But there is a performance trade off. If you carefully analyze, you realize that synchronization is only needed during initialization of singleton instance, to prevent creating another instance of Singleton. All other invocations determine that instance is non-null and return it. Multiple threads can safely execute concurrently on all invocations except the first. However, because the method is synchronized, you pay the cost of synchronization for every invocation of the method, even though it is only required on the first invocation.



#### Double-checked locking


 




With double checked locking, we check if instance is created. If not, then we synchronize creating the object instance.



#### Disadvantage of double checking


This solution will not work in Java 1.4 or earlier. Many JVMs in Java version 1.4 and earlier contains implementation of the volatile keyword that allow improper synchronization of double checked locking.



#### Another version of Thread-safe Lazy Initialization


 







## Special Mentions in above two implementations




### Serializable


To make a singleton class serializable, it is not sufficient merely to add implements Serializable to its declaration. To maintain the singleton guarantee, you have to declare all instance fields transient and provide a readResolve method. Otherwise, each time a serialized instance is deserialized, a new instance will be created
readResolve works only when all fields are transient.



### Cloneable


Preferred way is to not to implement Cloneable interface since why do we need another copy of singleton object.



## Enum Singleton


Enum Singletons are relatively new concept and in practice from Java 5 onwards after introduction of Enum as keyword.



### Advantages


easy to write when compared to lazy inititialization approach
Enum Singletons handle serialization by default
Creation of enum instance is thread safe.

 
 





A single-element enum type is the best way to implement a singleton.
