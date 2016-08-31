---
author: sureshsajja
comments: true
date: 2012-12-12 00:11:41+00:00
layout: post
link: http://coderevisited.com/finding-the-intersection-point-of-two-linked-lists/
slug: finding-the-intersection-point-of-two-linked-lists
title: Finding the intersection point of two linked lists
wordpress_id: 318
categories:
- Algorithms
- Interview
- Java
tags:
- Linked list
---

## Finding the intersection point of two linked lists


Suppose there are two linked lists both of which intersect at some point and become a single linked list. The head pointers of both the lists are known. The number of nodes in each of the list before they intersect are unknown. we need to find that intersection point.

**Brute-Force approach**
Compare every node pointer in the first list with every other node pointer in the second list by which matching node pointers will lead us to the intersecting node.
Time complexity will be O(mn) which is not effective. 

**Efficient approach**
Find lengths of both lists
Calculate difference of the lengths, move that many steps in longer list
move in both lists in parallel until links to next node match. That is the intersecting point.
Time Complexity of this approach is O(max(m,n))

Sample Java code to create two linked lists, to create intersecting point between two linked lists, to find the intersecting point.
 
 





Check [here](https://github.com/sureshsajja/CodeRevisited/tree/master/src/com/coderevisited/linkedlists) for complete code




