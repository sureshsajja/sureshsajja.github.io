---
author: sureshsajja
comments: true
date: 2012-07-07 15:25:36+00:00
layout: post
#link: http://coderevisited.com/selection-sort/
slug: selection-sort
title: Selection Sort
wordpress_id: 9
categories:
- Algorithms
tags:
- Selection sort
- sorting
image:
  feature: abstract-1.jpg
---

Selection sort is an in-place comparison sort with O(n^2) time complexity. It's not stable sort.



### Algorithm of selection sort


Find the smallest element in the array.
Swap it with the value in the first position
Find next smallest entry and swap it with second entry. [ Continue in this way until the entire array is sorted.]



### Analysis of selection sort


In Selection sort, after k passes through the array, the first k elements are in sorted order. To find smallest element in the array, it requires scanning all n elements, thus n-1 comparisons are required for first pass. It requires n-2 comparisons for second pass, n-3 for third pass and so on. Each of these scans requires one swap.
Total comparisons = (n-1)+(n-2)+(n-3)+ .............+2+1 = (n-1)n/2
Total number of exchanges = n
**Sample code in java for selection sort**
 

    
    public static void sort(int[] array) {
    		for (int i = 0; i < array.length; i++) {
    			int min_index = i;
    			for (int j = i + 1; j < array.length; j++) {
    				if (array[j] < array[min_index]) {
    					min_index = j;
    				}
    				if (min_index != i) {
    					int temp = array[i];
    					array[i] = array[min_index];
    					array[min_index] = temp;
    				}
    			}
    		}
    	}



**Questions**
What is the maximum number of exchanges involving any particular item during selection sort? What is the average number of exchanges involving an item?
The average number of exchanges is exactly 1 because there are exactly n exchanges and n items. The maximum number of exchanges is n. 

A clerk at a shipping company is charged with the task of rearranging a number of large crates in order of the time they are to be shipped out. Thus, the cost of compares is very low (just look at the labels) relative to the cost of exchanges (move the crates). The warehouse is nearly full: there is extra space sufficient to hold any one of the crates, but not two. Which sorting method should the clerk use? (among Selection sort, Insertion sort)
use selection sort because it minimizes the number of exchanges.
