---
layout: post
title: Atomic variables
description: How to implement Atomic counter in java, using atomic variables in Java
comments: true
categories:
- Java
tags:
- Java
author: sureshsajja
---

This post will demonstrate how to code wait-free, lock-free programming using Atomic variable classes that were added to JDK1.5.

The traditional way to coordinate access to shared fields in the Java language is to use synchronization, ensuring that all access to shared fields is done holding the appropriate lock. With synchronization, you are assured that whichever thread holds the lock that protects a given set of variables will have exclusive access to those variables, and changes to those variables will become visible to other threads when they subsequently acquire the lock.

Before JDK1.5, if we want to build a thread safe counter with get(), increment(), decrement() methods, each method needs to be synchronized to ensure that no updates are lost, and that all threads see the most recent value of the counter.


#### Example code 

{% gist 949fdd412e14b8966f6a91a5fd18d614 %}


### But What's wrong with traditional approach

To execute any method, each thread has to acquire lock on object first.
If the lock is heavily contended (threads frequently ask to acquire the lock when it is already held by another thread), throughput can suffer, as contended synchronization can be quite expensive.  

Another problem with lock-based algorithms is that if a thread holding a lock is delayed (due to a page fault, scheduling delay, or other unexpected delay), then no thread requiring that lock may make progress. 

### Atomic Variables

In JDK5.0, a set of toolkit classes are introduced (java.util.concurrent.atomic) to support wait-free, lock-free programming
The atomic variable classes can be thought of as a generalization of volatile variables, extending the concept of volatile variables to support atomic conditional compare-and-set updates.
Reads and writes of atomic variables have the same memory semantics as read and write access to volatile variables.Operations on atomic variables get turned into the hardware primitives that the platform provides for concurrent access, such as compare-and-set.

For more information read package description [here](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/atomic/package-summary.html#package_description)


#### Example code using AtomicInteger

{% gist 03f29e54153e8e9bdcdb6ea78c793501 %}