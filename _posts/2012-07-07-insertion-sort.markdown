---
author: sureshsajja
comments: true
date: 2012-07-07 16:47:12+00:00
layout: post
link: http://coderevisited.com/insertion-sort/
slug: insertion-sort
title: Insertion Sort
wordpress_id: 27
categories:
- Algorithms
tags:
- Insertion sort
- sorting
---

Insertion sort builds sorted array one item at a time. Though it is less efficient on large data inputs, it has it's own advantages.



	
  1. Efficient on small data sets.

	
  2. Adaptive for substantially sorted data sets

	
  3. Stable, keeps relative of elements with same keys

	
  4. In-place, only requires a constant amount of O(1)  of additional memory space





### Algorithm of insertion sort


Every repetition of insertion sort removes an element from the input data, inserting it into the correct position in the already-sorted list, until no input elements remain. Sorting is typically done in-place. The resulting array after k iterations has the property where the first k + 1 entries are sorted.



### Analysis of insertion sort


**Best Case:** Array is already sorted. So no swapping of elements are required. It required n-1 comparisons (condition of inner loop, body never executed)

**Worst Case:** Array is sorted in reverse order. (So each item has to be moved to the front of the array)
No of comparisons: 0+1+2+3+......+n-1 = n(n-1)/2
No of swaps:n(n-1)/2

**Average Case**: For randomly ordered arrays
No of comparisons: ~n(n-1)/4
No of swaps: ~n(n-1)/4

**Example Code in Java** 

    
    public static void sort(int[] array) {
    	for (int i = 1; i < array.length; i++) {
    		for (int j = i; j > 0 && (array[j] < array[j - 1]); j--) {
    			int temp = array[j];
    			array[j] = array[j-1];
    			array[j-1] = temp;
    		}
    	}
    }





### Variations of insertion sort algorithm


Notice that we have a double test for the inner loop.
 

    
    j > 0 && (array[j] < array[j - 1]



The first condition is to ensure that we don’t run off the beginning of the array, and the second one is to stop the loop once we reach the correct spot to insert the item. we can avoid an index-out-of-bounds test in the inner loop by first putting the smallest item into position. The item that enables the test to be eliminated is known as a sentinel.
**Example Code for sort with sentinel**
 

    
    public static void sortWithSentinal(int[] array) {
    	for (int i = array.length - 1; i > 0; i--) {
    		if (array[i] < array[i - 1]) {
    			int temp = array[i];
    			array[i] = array[i - 1];
    			array[i - 1] = temp;
    		}
    	}
    
    	for (int i = 2; i < array.length; i++) {
    		for (int j = i; array[j] < array[j - 1]; j--) {
    			int temp = array[j];
    			array[j] = array[j - 1];
    			array[j - 1] = temp;
    		}
    	}
    }





### Questions



Which sort algorithm runs fastest for an array with all keys identical, selection sort or insertion sort?
Insertion sort runs in linear time when all keys are equal.

Suppose that we use insertion sort on a randomly ordered array where items have only one of three key values. Is the running time linear, quadratic, or something in between?
Quadratic
