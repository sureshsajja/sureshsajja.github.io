---
author: sureshsajja
comments: true
date: 2012-12-05 01:37:12+00:00
layout: post
#link: http://coderevisited.com/kth-node-from-end-of-a-linked-list/
slug: kth-node-from-end-of-a-linked-list
title: How to find kth node from end of a linked list?
wordpress_id: 276
categories:
- Algorithms
- Interview
- Java
tags:
- Linked list
image:
  feature: abstract-1.jpg
---

### kth node from end of a linked list


Finding kth node from end of a linked list is one of the classic problems related to linked list.
Suppose, if we know the size of the list as N, kth from the end will eventually be the (N-k)th node from the beginning.
But in most of the cases, size of the linked list is not known.
**simple and naive approach**



	
  * Find size of the linked list in the first scan

	
  * compute N-k th node in the second scan


**Efficient approach**
Not surprisingly, this problem can be solved in one scan of linked list.



	
  * use two pointers, ptr1, ptr2

	
  * start ptr2 from head and move k nodes. Initialize ptr1 to head

	
  * From now on move ptr1 and ptr2 until ptr2 reaches end of the list

	
  * As a result ptr1 points to kth node from end of the linked list.


 




Click [here](https://github.com/sureshsajja/CodeRevisited/tree/master/src/com/coderevisited/linkedlists) for complete code
