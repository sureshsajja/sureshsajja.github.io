---
layout: post
title: How to find kth node from end of a linked list?
description: kth node from end of a linked list, Java code to kth node from end of a linked list, linked list
comments: true
categories:
- Interview
- Java
- Linked list
tags:
- java
- Linked list
author: sureshsajja
---

### kth node from end of a linked list

Finding kth node from end of a linked list is one of the classic problems related to linked list.

Suppose, if we know the size of the list as N, kth from the end will eventually be the (N-k)th node from the beginning.
But in most of the cases, size of the linked list is not known.

How do we solve when size of linked list is not known?

**simple and naive approach**  
* Find size of the linked list in the first scan
* compute N-k th node in the second scan

**Efficient approach**  
Not surprisingly, this problem can be solved in one scan of linked list.

* use two pointers, ptr1, ptr2
* start ptr2 from head and move k nodes. Initialize ptr1 to head
* From now on move ptr1 and ptr2 until ptr2 reaches end of the list
* As a result ptr1 points to kth node from end of the linked list.


### Sample Code in Java

{% gist f81bb80f823e7eed87aab0c2dadde013 %}


This problem can be solved using recursive approach. 

{% gist 863e5ab0d51b22318a9c68fad41b2bc9 %}

Click [here](https://github.com/sureshsajja/CodeRevisited/tree/master/src/com/coderevisited/linkedlists) for complete code
