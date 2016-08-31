---
author: sureshsajja
comments: true
date: 2012-09-13 11:45:35+00:00
layout: post
link: http://coderevisited.com/2-sum-problem/
slug: 2-sum-problem
title: 2-SUM Problem
wordpress_id: 120
categories:
- Algorithms
- Interview
---

## Problem


Given an array of N distinct integers, write a program to find **if there exists any pair of integers whose sum is x**. This is popularly addressed as 2-SUM problem.


### Variation


Given an array of N distinct integers, write a program to find **all pair of integers whose sum is x**, x can be zero or any constant.


### Algorithm





	
  * Sort the array A[] in nlogn time

	
  * Assign two pointers, one(start) points to the start of array, second(end) points to the end of array. Repeat the following until you find a pair, else no pairs whose sum is x


`if (A[start]+A[end] == x) return start,end
if(sum > x) end--
if(sum < x)start++`

**Java code for finding if f there exists any pair of integers whose sum is x**

    
    public static void findTwoSum(int[] A, int x) {
    	Arrays.sort(A);		
    	int start = 0;
    	int end = A.length - 1;
    	boolean found = false;
    	while (!found && start < end) {
    		if (A[start] + A[end] == x)
    			found = true;
    		else if (A[start] + A[end] > x)
    			end--;
    		else if (A[start] + A[end] < x)
    			start++;
    	}
    	if (found)
    		System.out.println("Sum " + x
    				+ " is found, values the making sum are " + A[start] + " , "
    				+ A[end]);
    	else
    		System.out.println("No pair exists whose sum is " + x);
    }




### To find all pairs:





	
  * Sort the array in nlogn time

	
  * for each integer i, check if there exists (x - i) by doing binary search


**Sample code**

    
    public static void findAllPairsWithSumX(int[] A, int x) {
    	Arrays.sort(A);
    	for (int i = 0; i < A.length - 1; i++) {
    		int tofind = x - A[i];
    		int returned = Arrays.binarySearch(A, i + 1, A.length, tofind);
    		if (returned > 0) {
    			System.out.println("Sum " + x
    					+ " is found, values the making sum are " + A[i]
    					+ " , " + A[returned]);
    		}
    		}
    }
