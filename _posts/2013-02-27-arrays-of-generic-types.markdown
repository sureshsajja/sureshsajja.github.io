---
layout: post
title: Arrays of Generic types
description: Create arrays of generic types, Covariant arrays
comments: true
categories:
- Java
tags:
- Java
author: sureshsajja
---

This post examines differences between arrays and generics and finds out how we can create arrays of generic types in Java.

### Let's start with Arrays

In Java, Arrays are covariant, which means that if B is a subtype of A, B[] is also subtype of A[].

#### Is there a problem here

Yes, there is a problem. Suppose, we have array of integers. We can assign these array of integers to array of numbers and put a double value as shown in the following program.

{% gist 13f05a87582ab94679c049a7f2d4f183 %}


Though this programs compiles successfully, It throws ArrayStoreException at runtime since arrays are reified and enforce their element types at runtime.
Though It is illegal that integer container can't be assigned to Number container, We had to wait till we run this program. It's hard to find bugs at runtime than compile time.

### Invariant generics

The following program perform the same operations as sated above.

{% gist 807184f0dcd3301b2f7b04a601b5fee9 %}


This program throws compile error. Generics are invariant which means that if B is a subtype of A, List<B> is not subtype of List<A>.
 
And also, Generics are implemented by erasure. This means that they enforce their type constraints only at compile time and discard (or erase) their element type information at runtime. Erasure is what allows generic types to interoperate freely with legacy code that does not use generics.
It is preferred to use generic list over arrays because any bugs will be surfaced at compile time.

As a consequence of the fact, arrays are covariant but generics are not, we are not allowed to create array of generic types unless the type argument is an unbounded wildcard.


### Why arrays of generic types


Suppose, if you want to implement ArrayList<E> as follows, 
 
```java 
public class MyArrayList<E> {
    
    private E[] elements;
    	
    public MyArrayList(int size){
    		elements = new E[size];
    }
}
```

But above code does not work. The compiler doesn't know what type E really represents, so it cannot instantiate an array of type E.

### How can we create arrays of generic types 

One workaround is to create an Object[] and then cast it (generating a warning) to E[]:

```java
elements = (E[])new Object[size]; 
```

Second approach is through reflection by passing a class literal (Foo.class) of element type into the constructor, so that the implementation could know, at runtime, the value of E.
 
```java
public MyArrayList(Class<E> elementType, int size){
    elements = (E[]) Array.newInstance(elementType, size);
}
```

