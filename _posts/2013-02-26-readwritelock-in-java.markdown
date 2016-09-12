---
layout: post
title: ReadWriteLock in Java
description: How to implement readwrite lock in java, Implement concurrent hashmap, ReadWriteLock in java
comments: true
categories:
- Java
tags:
- Java
author: sureshsajja
---

The following post demonstrates ReadWriteLock semantics in java

### Problem

Readers-Writers problem in one of the classic synchronization problems in computer science.

Suppose, there is a shared resource and many threads, some of them for reading the data from shared resources and some of them for writing data.
our aim is to design a solution with the following constrains: 

* No thread may access the shared resource for reading or writing while another process is in the act of writing to it 
* No reader shall be kept waiting if the share is currently opened for reading 

Addition constraint based on writer's preference

No writer, once added to the queue, shall be kept waiting longer than absolutely necessary 
 
### How to implement

One of the traditional ways of implementations is to protect shared resource with built in synchronization. All access to shared resource requires an appropriate lock to be acquired. With built in synchronization, only one thread that holds the lock have access to shared resource.  
 
But if one reader acquired lock and is accessing shared resource, It is not possible for another reader to acquire lock and access the resource.  Hence, Traditional synchronization block/methods are not an option here.  
 

### ReadWriteLock

Java 5.0 introduces ReadWriteLock that solves this problem. ReadWriteLock maintain a pair of locks, one is associated for reading and another for writing.
Read Java doc [here](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/locks/ReadWriteLock.html).

The following code demonstrates how to use ReadWriteLock for achieving ConcurrentHashMap functionality. 
 
{% gist 4a3e2810d24d0e34e02a03d75ae1a7d0 %}

### Code for testing

{% gist 523fbaa307d51dd40f2c348a4ea9d918 %}



