---
author: sureshsajja
comments: true
date: 2013-02-24 07:17:06+00:00
layout: post
link: http://coderevisited.com/thread-pool-in-java/
slug: thread-pool-in-java
title: Thread Pool in Java
wordpress_id: 398
categories:
- Java
---

This post will talk about high level overview of Thread pools in Java and how they can be created. It also shows one sample implementation (not production quality) to demonstrate Thread pool in java. 



### What is Thread Pool?


It's a pool of worker threads with life cycle as follows:
1. Get a new task to execute
2. Execute it
3. Go back to waiting for next task



### Why Thread Pools?


In many server applications, we may want to process each client request in parallel. For that matter, we can choose traditional approach of creating one thread per request. 



#### Disadvantage of one thread per task approach





	
  * The overhead of creating a new thread for each request is significant. Server that processing requests can spend more time and consume more system resources in creating and destroying threads than it would processing actual client requests. 

	
  * Creating too many threads in one JVM can cause the system to run out of memory or thrash due to excessive memory consumption.



For example, the following sample code has a producer task to put an integer to queue and a consumer task to take one integer from queue. when running in loop, each time code creates a new thread to perform task. It creates 600 threads for short lived tasks. 

 



 



#### Num of threads created when i ran above program in my machine:


[![Image 24-02-13 at 10.03 AM](http://coderevisited.com/wp-content/uploads/2013/02/Image-24-02-13-at-10.03-AM.png)](http://coderevisited.com/wp-content/uploads/2013/02/Image-24-02-13-at-10.03-AM.png)

To prevent resource thrashing, server applications need some means of limiting how many requests are being processed at any given time. A thread pool offers a solution to both the problem of thread life-cycle overhead and the problem of resource thrashing. 



####  How to Create a Thread pool 

#### 



	
  * Create n threads. Call them as workers. 



  * For each worker, Implement run method with two constraints 1. Wait for a task on a queue 2. Execute the task and go back to waiting state.



  * Expose addTask method that adds a task to that task Queue. 





####  Code for sample Thread pool in java 


 
 




 



#### Modified code for producerConsumer to use Thread pool 



In the following code, Instead of creating a new thread each time, we submit a new task to the created thread pool.
 



 



#### Num of threads created when i ran above program in my machine:


[![Image 24-02-13 at 10.33 AM](http://coderevisited.com/wp-content/uploads/2013/02/Image-24-02-13-at-10.33-AM.png)](http://coderevisited.com/wp-content/uploads/2013/02/Image-24-02-13-at-10.33-AM.png)



### Note


Above Thread pool implementation is just for illustrative purpose. Please look at java.util.concurrent package of JDK 1.5 and above for more sophisticated Thread Pools implementations.
