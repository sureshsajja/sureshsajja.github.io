---
layout: post
title: Merge sort
description: Merge sort in java. Example code of Merge sort in Java, Best case, worst case, average case analysis of Merge sort. Top down merge sort. Bottom up merge sort
comments: true
categories:
- Algorithms
tags:
- Merge sort
- sorting
author: sureshsajja
---

Merge sort is based in an algorithmic design pattern called **divide and conquer**.
Most implementations produce a stable sort, keeps relative order of elements with equal keys.

Merge sort is an asymptotically optimal compare-based sorting algorithm.
It guarantees to sort an array of N items in time proportional to \\( NlogN \\), no matter what the input.
Its prime disadvantage is that common implementations doesn't sort in place, uses extra space proportional to N.


## Algorithm of merge sort

* Divide the unsorted array with size N into N sub arrays, each containing 1 element (an array of 1 element is considered sorted).
* Repeatedly Merge sub arrays to produce sorted array.

There are two common variations in merge sort implementation. 
1. Top down approach 
2. Bottom up approach

## Top-down implementation

Top down merge sort algorithm recursively divides the array into sub-arrays, then merges arrays during returns back up the call chain.

### Sample code in java

{% gist 45072917d4f7229152b3c264e539fa6a %}

### Improvements

* using insertion sort for small subarrays can improve performance
* Avoid calling merge operation if the array is already in sorted order, compare last element of first half with first element of second half  
* Eliminate copy to auxiliary array, Save time (but not space) by switching the role of the input and auxiliary array in each recursive call.

### Sample code in java for merge sort with improvements

{% gist 7456047e950aa470d544c08ecc4f4c08 %}

## Bottom-up implementation (non-recursive)

The main idea of the bottom-up merge sort is to sort the array in a sequence of passes.

Merge n subarrays of size 1 in the first pass, then do a second pass to merge those arrays in pairs, and so forth, continuing until we do a merge that encompasses the whole array.

### Sample Code in java

{% gist 3e0ac49b9a9e97871ee80e319ee14f93 %}
