---
layout: post
title: 2-SUM Problem
description: find if there exists any pair of integers whose sum is x, find all pair of integers whose sum is x
comments: true
categories:
- Algorithms
- Interview
tags:
- Algorithms
- two sum
author: sureshsajja
---

## Problem

Given an array of N distinct integers, write a program to find **if there exists any pair of integers whose sum is x**. 
This is popularly addressed as 2-SUM problem.


### Variation

Given an array of N distinct integers, write a program to find **all pair of integers whose sum is x**, x can be zero or any constant.

### Algorithm

* Sort the array A[] in \\( nlogn \\) time
* Assign two pointers, one(start) points to the start of array, second(end) points to the end of array.
Repeat the following until you find a pair, else no pairs whose sum is x

```
if (A[start]+A[end] == x) return start,end
if(sum > x) end--
if(sum < x)start++
```

**Java code for finding if f there exists any pair of integers whose sum is x**

{% gist 68ab77908cb00815b07847feb375d453 %}


### To find all pairs:

* Sort the array in nlogn time
* for each integer i, check if there exists (x - i) by doing binary search


**Sample code**

{% gist ff4eb242bbf7beba859c6224d239590c %}
