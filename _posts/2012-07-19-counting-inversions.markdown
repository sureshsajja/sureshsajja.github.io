---
layout: post
title: Counting inversions
description: Counting inversions in the given array. counting out of order keys in the array
comments: true
categories:
- Algorithms
tags:
- Merge sort
- sorting
- counting inversions
author: sureshsajja
---

An inversion is a pair of keys that are out of order in the array.

* Number of inversions = number of pairs (i,j) of array indices with (A[i] > A[j]) 
* Maximum possible number of inversions that an N element array can have = \\( n*(n-1)/2 \\)


Merge sort algorithm naturally uncovers inversions. The following is one of the ways of counting inversions in \\( O(nlogn) \\) time.

{% gist 135a19ec96737359a7a10a07c378ab64 %}
