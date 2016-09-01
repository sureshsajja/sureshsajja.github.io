---
author: sureshsajja
comments: true
date: 2014-10-02 15:06:41+00:00
layout: post
#link: http://coderevisited.com/replace-every-element-with-next-greatest/
slug: replace-every-element-with-next-greatest
title: Replace Every Element With Next Greatest
wordpress_id: 503
categories:
- Algorithms
- Java
image:
  feature: abstract-1.jpg
---

## Problem: Replace Every Element With Next Greatest


Given an array of integers, replace every element with next greatest element.Since there is no element next to the last element, replace it with -1. In an array of a1 to aN, replace ai with max (ai+1 …. aN) where $latex 0 \le i \le N-2$ and aN-1 with -1


## Solution


Consider the array with only positive integers. Initially set maximum element as -1. Start from the N-1th element, move to the left side one by one, and keep track of the maximum element. Replace every element with the maximum element.

What if our array contains negative integers too?


### Modified solution


Initially set maximum element as Integer.MIN_VALUE. Start from the N-1th element, move to the left side one by one, and keep track of the maximum element. Replace every element with the maximum element. And Finally set the last element to -1.


## Sample Code



