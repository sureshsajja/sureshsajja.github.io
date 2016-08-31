---
author: sureshsajja
comments: true
date: 2013-02-26 18:32:39+00:00
layout: post
link: http://coderevisited.com/readwritelock-in-java/
slug: readwritelock-in-java
title: ReadWriteLock in Java
wordpress_id: 409
categories:
- Java
---

The following post demonstrates ReadWriteLock semantics in java


### Problem


Readers-Writers problem in one of the classic synchronization problems in computer science.  
Suppose, there is a shared resource and many threads, some of them for reading the data from shared resources and some of them for writing data.  
our aim is to design a solution with the following constrains: 
No thread may access the shared resource for reading or writing while another process is in the act of writing to it 
No reader shall be kept waiting if the share is currently opened for reading 
Addition constraint based on writer's preference  
No writer, once added to the queue, shall be kept waiting longer than absolutely necessary 
 


### How to implement




#### Built in synchronization blocks/methods


One of the traditional ways of implementations is to protect shared resource with built in synchronization. All access to shared resource requires an appropriate lock to be acquired. With built in synchronization, only one thread that holds the lock have access to shared resource.  
 
But if one reader acquired lock and is accessing shared resource, It is not possible for another reader to acquire lock and access the resource.  Hence, Traditional synchronization block/methods are not an option here.  
 


### ReadWriteLock


Java 5.0 introduces ReadWriteLock that solves this problem. ReadWriteLock maintain a pair of locks, one is associated for reading and another for writing.  Read Java doc [here](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/locks/ReadWriteLock.html).

The following code demonstrates how to use ReadWriteLock for achieving ConcurrentHashMap functionality. 
 

    
    package com.code.revisited.concurrent.locks;
    
    import java.util.HashMap;
    import java.util.Map;
    import java.util.concurrent.locks.Lock;
    import java.util.concurrent.locks.ReadWriteLock;
    import java.util.concurrent.locks.ReentrantReadWriteLock;
    
    /**
     * This class demonstrates ReadWriteLock that was introduced in Java 5.0
     * @author sureshsajja
     *
     */
    public class MyConcurrentHashMap<K,V> {
    
    	private Map<K,V> hashMap = new HashMap<>();
    	//non-fair read write lock
    	private final ReadWriteLock lock = new ReentrantReadWriteLock();
    	private final Lock readLock = lock.readLock();
    	private final Lock writeLock = lock.writeLock();
    	
    	public MyConcurrentHashMap(){
    		
    	}
    	
    	public void put(K key, V value){
    		writeLock.lock();
    		try{
    			hashMap.put(key, value);
    		}finally{
    			writeLock.unlock();
    		}
    	}
    	
    	public V get(K key){
    		readLock.lock();
    		try{
    			return hashMap.get(key);
    		}finally{
    			readLock.unlock();
    		}
    	}
    	
    	public V remove(K key){
    		writeLock.lock();
    		try{
    			return hashMap.remove(key);
    		}finally{
    			writeLock.unlock();
    		}
    	}
    	
    	public boolean containsKey(K key){
    		readLock.lock();
    		try{
    		return hashMap.containsKey(key);
    		}finally{
    			readLock.unlock();
    		}
    	}
    
    }
    



