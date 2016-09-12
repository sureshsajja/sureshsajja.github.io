---
layout: post
title: Replace Every Element With Next Greatest
description: Replace Every Element With Next Greatest, Java code for Replace Every Element With Next Greatest, Algorithm for replacing every element With Next Greatest
comments: true
categories:
- Algorithms
tags:
- Algorithms
author: sureshsajja
---

## Problem: Replace Every Element With Next Greatest

Given an array of integers, replace every element with next greatest element.Since there is no element next to the last element, replace it with -1.

In an array of a1 to aN, replace ai with \\( max (ai+1 …. aN) \\) where  \\( 0 < i < N \\) and aN with -1


## Solution

* Consider the array with only positive integers. 
* Initially set maximum element as -1. 
* Start from the N-1th element, move to the left side one by one, and keep track of the maximum element. 
* Replace every element with the maximum element.

What if our array contains negative integers too?

### Modified solution

* Initially set maximum element as Integer.MIN_VALUE. 
* Start from the N-1th element, move to the left side one by one, and keep track of the maximum element. 
* Replace every element with the maximum element. And Finally set the last element to -1.

## Sample Code

{% gist c0eab10f847ec3ca2c245ca3bdbcffc7 %}


Check [here](https://github.com/sureshsajja/CodingProblems/blob/master/src/main/java/com/coderevisited/arrays/ReplaceWithNextGreatest.java) for complete code.