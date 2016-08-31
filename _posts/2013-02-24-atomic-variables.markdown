---
author: sureshsajja
comments: true
date: 2013-02-24 18:21:27+00:00
layout: post
#link: http://coderevisited.com/atomic-variables/
slug: atomic-variables
title: Atomic variables
wordpress_id: 406
categories:
- Java
---

This post will demonstrate how to code wait-free, lock-free programming using Atomic variable classes that were added to JDK1.5.

The traditional way to coordinate access to shared fields in the Java language is to use synchronization, ensuring that all access to shared fields is done holding the appropriate lock. With synchronization, you are assured that whichever thread holds the lock that protects a given set of variables will have exclusive access to those variables, and changes to those variables will become visible to other threads when they subsequently acquire the lock.

Before JDK1.5, if we want to build a thread safe counter with get(), increment(), decrement() methods, each method needs to be synchronized to ensure that no updates are lost, and that all threads see the most recent value of the counter.



#### Example code 


 

    
    package com.code.revisited.concurrent.counter;
    
    public interface Counter {
    
    	int getCount();
    	
    	int increment();
    	
    	int decrement();
    }
    



 

    
    package com.code.revisited.concurrent.counter;
    
    public class SynchronizedCounter {
    
    	private int count;
    
    	public synchronized int getCount() {
    		return count;
    	}
    
    	public synchronized int increment() {
    		return ++count;
    	}
    
    	public synchronized int decrement() {
    		return --count;
    	}
    
    }
    





### But What's wrong with traditional approach


To execute any method, each thread has to acquire lock on object first. If the lock is heavily contended (threads frequently ask to acquire the lock when it is already held by another thread), throughput can suffer, as contended synchronization can be quite expensive.  
Another problem with lock-based algorithms is that if a thread holding a lock is delayed (due to a page fault, scheduling delay, or other unexpected delay), then no thread requiring that lock may make progress. 



### Atomic Variables


In JDK5.0, a set of toolkit classes are introduced (java.util.concurrent.atomic) to support wait-free, lock-free programming
The atomic variable classes can be thought of as a generalization of volatile variables, extending the concept of volatile variables to support atomic conditional compare-and-set updates. Reads and writes of atomic variables have the same memory semantics as read and write access to volatile variables.Operations on atomic variables get turned into the hardware primitives that the platform provides for concurrent access, such as compare-and-set.

For more information read package description [here](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/atomic/package-summary.html#package_description)



#### Example code using AtomicInteger


 

    
    package com.code.revisited.concurrent.counter;
    
    import java.util.concurrent.atomic.AtomicInteger;
    
    public class AtomicCounter implements Counter{
    
    	private final AtomicInteger counter = new AtomicInteger();
    
    	@Override
    	public int getCount() {
    		return counter.get();
    	}
    	
    	@Override
    	public final int increment(){
    		return counter.incrementAndGet();
    	}
    	
    	@Override
    	public final int decrement(){
    		return counter.decrementAndGet();
    	}
    
    }



 

    
    package com.code.revisited.concurrent.counter;
    
    
    public class TestCounter {
    	
    	private Counter counter;
    
    	public static void main(String[] args) {
    
    		TestCounter counter = new TestCounter();
    		counter.init();
    		
    		
    	}
    
    	private void init() {
    		
    		counter = new AtomicCounter();
    		
    		for (int i = 0; i < 100; i++) {
    			new Thread(new Increment()).start();
    			new Thread(new Decrement()).start();
    			new Thread(new Increment()).start();
    		}
    		
    		try {
    			Thread.sleep(10000);
    		} catch (InterruptedException e) {
    			e.printStackTrace();
    		}
    
    		System.out.println("Final Counter " + counter.getCount());
    
    	}
    
    	private class Increment implements Runnable {
    
    		@Override
    		public void run() {
    			try {
    				Thread.sleep(10);
    			} catch (InterruptedException e) {
    				e.printStackTrace();
    			}
    			counter.increment();
    		}
    	}
    
    	private class Decrement implements Runnable {
    		@Override
    		public void run() {
    			try {
    				Thread.sleep(10);
    			} catch (InterruptedException e) {
    				e.printStackTrace();
    			}
    			counter.decrement();
    		}
    	}
    
    }
    



