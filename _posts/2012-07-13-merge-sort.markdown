---
author: sureshsajja
comments: true
date: 2012-07-13 14:30:18+00:00
layout: post
link: http://coderevisited.com/merge-sort/
slug: merge-sort
title: Merge sort
wordpress_id: 65
categories:
- Algorithms
tags:
- merge sort
- sorting
---

Merge sort is based in an algorithmic design pattern called **divide and conquer**.  Most implementations produce a stable sort, keeps relative order of elements with equal keys.

Mergesort is an asymptotically optimal compare-based sorting algorithm. It guarantees to sort an array of N items in time proportional to [latex]NlogN[/latex], no matter what the input. Its prime disadvantage is that common implementations doesn't sort in place, uses extra space proportional to N.


## Algorithm of merge sort





	
  * Divide the unsorted array with size N into N sub arrays, each containing 1 element (an array of 1 element is considered sorted).

	
  * Repeatedly Merge sub arrays to produce sorted array.



There are two common variations in merge sort implementation. 1. Top down approach 2. Bottom up approach


## Top-down implementation


Top down merge sort algorithm recursively divides the array into sub-arrays, then merges arrays during returns back up the call chain.



### Sample code in java


[java]

import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class MergeSortTopDown {

	public static <T extends Comparable<? super T>> void mergeSort(T[] toBeSorted) {
		mergeSort(toBeSorted, 0, toBeSorted.length - 1);
	}

	private static <T extends Comparable<? super T>> void mergeSort(T[] toBeSorted, int low, int high) {
		if (low < high) {
			// why unsigned right shift???
			// Read more about this
			// http://googleresearch.blogspot.in/2006/06/extra-extra-read-all-about-it-nearly.html
			int mid = (low + high) >>> 1;
			mergeSort(toBeSorted, low, mid);
			mergeSort(toBeSorted, mid + 1, high);
			merge(toBeSorted, low, mid, high);
		}
	}

	private static <T extends Comparable<? super T>> void merge(T[] toBeSorted, int low, int mid, int high) {
		//Copy to an auxiliary array 
		T[] aux = toBeSorted.clone();
		int index_firstHalf = low, index_secondHalf = mid + 1;
		for (int loopIndex = low; loopIndex <= high; loopIndex++) {

			if (index_firstHalf > mid)
				// First half is merged, add second half one by one
				toBeSorted[loopIndex] = aux[index_secondHalf++];
			else if (index_secondHalf > high)
				// Second half is merged, add first half one by one
				toBeSorted[loopIndex] = aux[index_firstHalf++];

			// else compare them, add whichever is lesser.
			else if (aux[index_firstHalf].compareTo(aux[index_secondHalf]) <= 0)
				toBeSorted[loopIndex] = aux[index_firstHalf++];
			else
				toBeSorted[loopIndex] = aux[index_secondHalf++];
		}
	}

	// Test program to read strings and sort them based on lexicographic
	// ordering
	public static void main(String[] args) {
		if (!(args.length == 1)) {
			System.out.println("correct usage : java MergeSortTopDown FileName");
			return;
		}

		try {
			String text = new Scanner(new File(args[0])).useDelimiter("\\A").next();
			String[] toBeSorted = text.trim().split("\\s+");
			mergeSort(toBeSorted);
			for (int i = 0; i < toBeSorted.length; i++) {
				System.out.println(toBeSorted[i]);
			}
		} catch (FileNotFoundException e) {
			System.out.println("File not found " + new File(args[0]).getAbsolutePath());
		}
	}

}

[/java]



### Improvements





	
  * using insertion sort for small subarrays can improve performance

	
  * Avoid calling merge operation if the array is already in sorted order, compare last element of first half with first element of second half  

        
  * Eliminate copy to auxiliary array, Save time (but not space) by switching the role of the input and auxiliary array in each recursive call.





### Sample code in java for merge sort with improvements



[java]


import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class MergeSortTopDownImproved {

	private static final int THRESHOLD = 7;

	public static <T extends Comparable<? super T>> void mergeSort(T[] toBeSorted) {
		// Copy to an auxiliary array
		T[] aux = toBeSorted.clone();
		mergeSort(toBeSorted, aux, 0, toBeSorted.length - 1);
	}

	private static <T extends Comparable<? super T>> void mergeSort(T[] toBeSorted, T[] aux, int low, int high) {

		// Improvement 1
		if (high <= low + THRESHOLD) {
			// For code check my post on insertion sort
			InsertionSortWithShifting.sort(toBeSorted);
			return;
		}

		int mid = (low + high) >>> 1;
		// Improvement 3
		mergeSort(aux, toBeSorted, low, mid);
		mergeSort(aux, toBeSorted, mid + 1, high);

		//Improvement 2
		if ((aux[mid]).compareTo(aux[mid + 1]) <= 0) {
			System.arraycopy(aux, low, toBeSorted, low, high - low + 1);
			return;
		}

		merge(toBeSorted, aux, low, mid, high);

	}

	private static <T extends Comparable<? super T>> void merge(T[] toBeSorted, T[] aux, int low, int mid, int high) {

		int index_firstHalf = low, index_secondHalf = mid + 1;
		for (int loopIndex = low; loopIndex <= high; loopIndex++) {

			if (index_firstHalf > mid)
				// First half is merged, add second half one by one
				toBeSorted[loopIndex] = aux[index_secondHalf++];
			else if (index_secondHalf > high)
				// Second half is merged, add first half one by one
				toBeSorted[loopIndex] = aux[index_firstHalf++];
			// else compare them, add whichever is lesser.
			else if (aux[index_firstHalf].compareTo(aux[index_secondHalf]) <= 0)
				toBeSorted[loopIndex] = aux[index_firstHalf++];
			else
				toBeSorted[loopIndex] = aux[index_secondHalf++];
		}
	}

	// Test program to read strings and sort them based on lexicographic
	// ordering
	public static void main(String[] args) {
		if (!(args.length == 1)) {
			System.out.println("correct usage : java MergeSortTopDownImproved FileName");
			return;
		}

		try {
			String text = new Scanner(new File(args[0])).useDelimiter("\\A").next();
			String[] toBeSorted = text.trim().split("\\s+");
			mergeSort(toBeSorted);
			for (int i = 0; i < toBeSorted.length; i++) {
				System.out.println(toBeSorted[i]);
			}
		} catch (FileNotFoundException e) {
			System.out.println("File not found " + new File(args[0]).getAbsolutePath());
		}
	}

}

[/java]



## Bottom-up implementation (non-recursive)



The main idea of the bottom-up merge sort is to sort the array in a sequence of passes. 
Merge n subarrays of size 1 in the first pass, then do a second pass to merge those arrays in pairs, and so forth, continuing until we do a merge that encompasses the whole array.



### Sample Code in java



[java]

private static void mergeSortBU(String[] toBeSorted) {

		// the outer loop checks the size of sub arrays that are being merged.
		// starting with 1, then 2, 4, 8 and so on....
		for (int arraySize = 1; arraySize < toBeSorted.length; arraySize = arraySize + arraySize) {

			// we need to extract two sub arrays of size = arraySize
			// for each iteration index jumps to 2*arraySize
			// loop termination: No of elements from index should be at least
			// equal to arraySize;
			for (int index = 0; index < toBeSorted.length - arraySize; index = index + arraySize + arraySize) {
				int low = index;
				int mid = index + arraySize - 1;
				int high = Math.min(index + arraySize + arraySize - 1, toBeSorted.length - 1);
				merge(toBeSorted, low, mid, high);
			}

		}

	}
[/java]
