---
author: sureshsajja
comments: true
date: 2013-02-15 14:04:25+00:00
layout: post
#link: http://coderevisited.com/boxing-and-unboxing-in-java/
slug: boxing-and-unboxing-in-java
title: Boxing and Unboxing in Java
wordpress_id: 325
categories:
- Java
image:
  feature: abstract-1.jpg
---

In Java programming language, there are eight primitive types and each of these has a corresponding library class of reference type. For example, there is a class java.lang.Integer that corresponds to primitive type int. These kinds of classes are called wrappers. The wrapper classes are immutable, we cannot change a wrapped value after the wrapper has been constructed. They are also final, so we cannot subclass them.
**Why do we have wrapper classes for primitive types?**



	
  * The wrapper classes are used whenever a primitive type needs to be treated as an Object. For example, the classes in the Collections API (like ArrayList) can only hold Object references.

	
  * To provide an assortment of utility functions for primitives. Most of these functions are related to various conversions: converting primitives to and from String objects, and converting primitives and String objects to and from different bases (or radix), such as binary, octal, and hexadecimal.


**What is Boxing and Unboxing?**
Conversion of a primitive type to the corresponding reference type is called boxing, such as an int to a java.lang.Integer. Conversion of the reference type to the corresponding primitive type is called unboxing, such as Byte to byte.
Since JDK 1.5, Conversion from primitive types to corresponding wrapper objects and vice versa can happen automatically.

Till JDK 1.4, if we want to create a List with integers, we have to explicitly wrap them as Integer. And we need to unwrap it to get int value.

    
    List ints = new ArrayList();
    ints.add(new Integer(5));
    ints.add(new Integer(6));
    
    int i = ((Integer)ints.get(0)).intValue();
    



As of JDK 1.5, it got better with Auto-boxing and genrics 

    
    List<Integer> ints = new ArrayList<Integer>();
    ints.add(5);
    ints.add(6);
    //Returns integer at index 0
    int i = ints.get(0);



**Unintentional auto-boxing**
Consider the following code for adding first 1 billion integers


    
    public class UnIntentionalObjectCreation {
    
    	private static Integer sumOfIntegerUptoN(Integer N) {
    		Integer sum = 0;
    		for (Integer i = 0; i < N; i++) {
    			sum += i;
    		}
    		return sum;
    	}
    
    	public static void main(String[] args) {
    		long start = System.currentTimeMillis();
    		sumOfIntegerUptoN(1000000000);
    		long end = System.currentTimeMillis();
    		System.out.println((end - start));
    	}
    
    }



The above code compiles and runs fine but it performs lot of unnecessary work. Each iteration of for loop unboxes sum and i, performs addition, boxes result to sum. 
When i run the above program on my computer, it took 7689 milli sec.

**Efficient version for sum of integers**

 

    
    public class SumOfIntegers {
    
    	private static int sumOfIntegerUptoN(int N) {
    		int sum = 0;
    		for (int i = 0; i < N; i++) {
    			sum += i;
    		}
    		return sum;
    	}
    
    	public static void main(String[] args) {
    		long start = System.currentTimeMillis();
    		sumOfIntegerUptoN(1000000000);
    		long end = System.currentTimeMillis();
    		System.out.println((end - start));
    	}
    
    }


Improved version took just 385 milli sec.
The lesson is clear that prefer primitives to boxed primitives, and watch out for unintentional auto-boxing

**Method overloading**

Suppose, we have following two methods

    
    
    private static void printSum(double N)
    private static void printSum(Integer N)
    



When you invoke printSum(10), what's get invoked? you may be expecting, printSum(Integer N) get's called. But that's not the case.
**Method resolution Rules**




	
  * The compiler attempts to locate the correct method without any boxing, unboxing, or vararg invocations. This will find any method that would have been invoked under Java 1.4 rules

	
  * If the first pass fails, the compiler tries method resolution again, this time allowing boxing and unboxing conversions. Methods with varargs are not considered in this pass


  * If the second pass fails, the compiler tries method resolution one last time, allowing boxing and unboxing, and also considers vararg methods



**NullPointerException**
 

    
    Integer i = null;
    int j = i; 


Though above code compiles successfully, we are adding null to a primitive type where NullPointerException is thrown.

**Final Note: Caching of immutable wrapper objects**
The Java specification indicates that certain primitives are always to be boxed into the same immutable wrapper objects. These objects are then cached and reused, with the expectation that these are commonly used objects. 
These special values are the boolean values true and false, all byte values, short and int values between -128 and 127, and any char in the range \u0000 to \u007F.
