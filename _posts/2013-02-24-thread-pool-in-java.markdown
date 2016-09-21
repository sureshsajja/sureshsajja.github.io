---
layout: post
title: Thread Pool in Java
description: How to implement Thread pool in java, Sample code for implementing thread pool, what is Thread pool, Why Thread pool,Disadvantage of one thread per task approach
comments: true
categories:
- Java
tags:
- Java
author: sureshsajja
---

This post will talk about high level overview of Thread pools in Java and how they can be created.
It also shows one sample implementation (not production quality) to demonstrate Thread pool in java. 

### What is Thread Pool?

It's a pool of worker threads with life cycle as follows:
1. Get a new task to execute
2. Execute it
3. Go back to waiting for next task

### Why Thread Pools?

In many server applications, we may want to process each client request in parallel.
For that matter, we can choose traditional approach of creating one thread per request. 

#### Disadvantage of one thread per task approach

* The overhead of creating a new thread for each request is significant.
Server that processing requests can spend more time and consume more system resources in creating and destroying threads than it would processing actual client requests. 
* Creating too many threads in one JVM can cause the system to run out of memory or thrash due to excessive memory consumption.

For example, the following sample code has a producer task to put an integer to queue and a consumer task to take one integer from queue.
when running in loop, each time code creates a new thread to perform task. It creates 600 threads for short lived tasks. 

{% gist 79a4eaff5e7a58b1d63a069d3b58acbd %}

#### Num of threads created when i ran above program in my machine:


![Num of threads without pool](/images/ThreadPool1.png)

To prevent resource thrashing, server applications need some means of limiting how many requests are being processed at any given time.
A thread pool offers a solution to both the problem of thread life-cycle overhead and the problem of resource thrashing. 


####  How to Create a Thread pool 

* Create n threads. Call them as workers. 
* For each worker, Implement run method with two constraints 1. Wait for a task on a queue 2. Execute the task and go back to waiting state.
* Expose addTask method that adds a task to that task Queue. 


####  Code for sample Thread pool in java 

{% gist 58373419234fe096f4057a43ec8b43e7 %} 

#### Modified code for producerConsumer to use Thread pool 

{% gist f7334ee4114dea5c15ac7269c8a31e4b %}

In the following code, Instead of creating a new thread each time, we submit a new task to the created thread pool.
 

#### Num of threads created when i ran above program in my machine:

![Num of threads with pool](/images/ThreadPool2.png)

### Note

Above Thread pool implementation is just for illustrative purpose.
Please look at java.util.concurrent package of JDK 1.5 and above for more sophisticated Thread Pools implementations.
