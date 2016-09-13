---
layout: post
title: How to detect if a linked list has a loop?
description: How to detect if a linked list has a loop?, Java code to detect if a linked list has a loop?, loop in a linked list.
comments: true
categories:
- Algorithms
- Interview
- Java
tags:
- Linked list
author: sureshsajja
---

### Detect if a linked list has a loop

This is one of the classic interview problems.

### The easiest approach

* Traverse through the entire list, keeping track of visited nodes
* on each node check if node is already visited: if yes, list has a loop else continue till end of the list.

If we can modify node of linked list, we can add "visited" boolean to each node so that visited is set when a node is visited for the first time.
But in most cases, it is not allowed to modify node structure. Then, we may need to maintain a separate HashSet to store the nodes that are visited.
Each time, when a new node is being visited, we need to verify if that node is already existed in the Set. It's pretty obvious that this runs in O(n) time complexity, O(n) space complexity.

### Efficient Algorithm

### Floyd's cycle detection algorithm

* The Tortoise (Slow ptr) and the Hare (fast ptr) start at the beginning of linked list.
* For each iteration, slow ptr moves one step and the fast ptr moves two steps.
* If there is a loop, fast ptr will go around the loop and meet with slow ptr.
* If there is no loop, the fast prt will get to the end of the list without meeting up with the slow ptr.

The following algorithm runs with O(n) time complexity and O(1) space complexity
we may think that fast ptr might just hop over slow ptr. It's not possible. Let say, fast ptr did hop over slow ptr where slow ptr is at position i and fast ptr is at i+1. In the previous step slow ptr is at i-1 position, fast ptr is at (i+1)-2 or i-1. That means, they would have met eventually.

### Sample Java code Floyd's algorithm
 
{% gist 08244a78cfef537783409bb14035977e %}


### Brent's Cycle detection Algorithm

This algorithm is based on Floyd‘s algorithm. It is more efficient (24-36% faster on average) than Floyd‘s but little complicated.

* Start ptr1 (Moving ptr), ptr2 (Stationary pointer) from beginning of the list
* ptr1 takes one step per iteration, If it is then at the same position as the ptr2, there is obviously a loop. If it reaches the end of the list, there is no loop.
* Teleport ptr2 position to ptr1 position occasionally. We start out waiting just 2 steps before teleportation, and we double that each time we move the ptr2.

### Sample java code using Brent's Cycle Detection Algorithm
 
{% gist 0c5a60ca56fe56ee6fd8597f79df1cf7 %}

### Length of the loop

This is an extension to the basic loop detection problem.

* Use Floyd's cycle detection algorithm to find if there is a loop.
* After finding the loop, Initialise slowptr to fastPtr.
* Keep moving slowptr and until it come back to fastptr, use a counter variable when moving slowptr.

### Sample Java code to find loop length
 
{% gist 4f40c1593375d63656ce463c245a1de9 %}

### Start Node of the Loop

* Use Floyd's cycle detection algorithm to find if there is a loop.
* After finding the loop, Initialise slowptr to head.
* Keep moving slowptr, fastptr by one position until they both meet

### Sample Java code to find start node of loop
 
{% gist 41000313c8a53aa1fd27aa73c1336585 %}

### Correctness of algorithm for finding start node of loop

Lets assume that linked list has a "non-looped" part of size k. When we apply Floyd's cycle detection algorithm, we know that after k steps,

* slowPtr will be at start of loop, and is 0 steps into the loop.
* fastPtr is k steps into the loop. since k might be much larger than loop_size, it is actually k%loop_size. Let's say it as p. So fastPtr is p steps into the loop.
* slowPtr is p steps behind fastPtr.
* fastPtr is loop_size - p steps behind slowPtr.
* fastPtr catches up to slowPtr at a rate of one step per unit of time.


From above points, after loop_size - p steps, fastPtr and slowPtr meet each other, which means they will be p steps before start of the loop.
Since p = k%loop_size ( k = p+M*loop_size), it is correct to say that, fastPtr, slowPtr are k steps from start node of the loop.
If we reinitialise slowPtr to head node and start moving both pointers one node at a time. they both meet at start node of the loop.

Click [here](https://github.com/sureshsajja/CodeRevisited/tree/master/src/com/coderevisited/linkedlists) for complete code.
