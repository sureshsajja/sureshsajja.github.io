---
author: sureshsajja
comments: true
date: 2013-02-27 05:44:01+00:00
layout: post
#link: http://coderevisited.com/arrays-of-generic-types/
slug: arrays-of-generic-types
title: Arrays of Generic types
wordpress_id: 412
categories:
- Java
---

This post examines differences between arrays and generics and finds out how we can create arrays of generic types in Java.


### Let's start with Arrays


In Java, Arrays are covariant, which means that if B is a subtype of A, B[] is also subtype of A[].


#### Is there a problem here


Yes, there is a problem. Suppose, we have array of integers. We can assign these array of integers to array of numbers and put a double value as shown in the following program.

    
    package com.code.revisited.generics;
    
    public class CovariantArrays {
    
    	/**
    	 * This program throws runtime error. Because arrays are covariant,
    	 * assigning integer array to number array is successful at compile time
    	 * But, arrays enforce their element types at runtime. ArrayStoreException
    	 * is thrown while adding double to integer array
    	 * 
    	 * @param args
    	 */
    	public static void main(String[] args) {
    
    		Integer[] intArray = new Integer[5];
    		Number[] objArray = intArray;
    		objArray[0] = 1.0;
    
    	}
    
    }


Though this programs compiles successfully, It throws ArrayStoreException at runtime since arrays are reified and enforce their element types at runtime.
Though It is illegal that integer container can't be assigned to Number container, We had to wait till we run this program. It's hard to find bugs at runtime than compile time.


### Invariant generics


The following program perform the same operations as sated above.

    
    package com.code.revisited.generics;
    
    import java.util.ArrayList;
    import java.util.List;
    
    /**
     * 
     * @author sureshsajja
     * 
     */
    public class InvariantGenerics {
    
    	/**
    	 * This program throws compile error. Because generics are invariant, List of integers are not
    	 * compatible with list of numbers
    	 * 
    	 * @param args
    	 */
    	public static void main(String[] args) {
    		List<Integer> listofInts = new ArrayList<Integer>();
    		List<Number> listOfNums = listofInts;
    		listOfNums.add(1.0);
    
    	}
    
    }


This program throws compile error. Generics are invariant which means that if B is a subtype of A, List<B> is not subtype of List<A>. And also, Generics are implemented by erasure. This means that they enforce their type constraints only at compile time and discard (or erase) their element type information at runtime. Erasure is what allows generic types to interoperate freely with legacy code that does not use generics.
It is preferred to use generic list over arrays because any bugs will be surfaced at compile time.

As a consequence of the fact, arrays are covariant but generics are not, we are not allowed to create array of generic types unless the type argument is an unbounded wildcard.


### Why arrays of generic types


Suppose, if you want to implement ArrayList<E> as follows, 
 

    
    public class MyArrayList<E> {
    
    	private E[] elements;
    	
    	public MyArrayList(int size){
    		elements = new E[size];
    	}
    
    }



But above code does not work. The compiler doesn't know what type E really represents, so it cannot instantiate an array of type E.



### How can we create arrays of generic types 



One workaround is to create an Object[] and then cast it (generating a warning) to E[]:


    
    
    
    elements = (E[])new Object[size]; 



Second approach is through reflection by passing a class literal (Foo.class) of element type into the constructor, so that the implementation could know, at runtime, the value of E.
 

    
    public MyArrayList(Class<E> elementType, int size){
    	elements = (E[]) Array.newInstance(elementType, size);
    }



/**
END
*/
