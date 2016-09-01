---
author: sureshsajja
comments: true
date: 2012-07-19 18:22:19+00:00
layout: post
#link: http://coderevisited.com/counting-inversions/
slug: counting-inversions
title: Counting inversions
wordpress_id: 92
categories:
- Algorithms
image:
  feature: abstract-1.jpg
---

An inversion is a pair of keys that are out of order in the array.



	
  * Number of inversions = number of pairs (i,j) of array indices with (A[i] > A[j]) 

	
  * Maximum possible number of inversions that an N element array can have = [latex]n*(n-1)/2[/latex]



Merge sort algorithm naturally uncovers inversions. The following is one of the ways of counting inversions in[latex] O(nlogn)[/latex] time.
[java]

import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class Inversions {

	public static long countInversions(int[] intArray) {
		// Copy to an auxiliary array
		int[] aux = intArray.clone();
		return count(intArray, aux, 0, intArray.length - 1);
	}

	private static long count(int[] intArray, int[] aux, int low, int high) {
		long inversions = 0;
		if (low < high) {

			int mid = (low + high) >>> 1;
			inversions = inversions + count(aux, intArray, low, mid);
			inversions = inversions + count(aux, intArray, mid + 1, high);

			if (aux[mid] <= aux[mid + 1]) {
				System.arraycopy(aux, low, intArray, low, high - low + 1);
				return inversions;
			}

			inversions = inversions + countSpiltInversions(intArray, aux, low, mid, high);
		}
		return inversions;

	}

	private static long countSpiltInversions(int[] intArray, int[] aux, int low, int mid, int high) {
		long inversions = 0;
		int index_firstHalf = low, index_secondHalf = mid + 1;
		for (int loopIndex = low; loopIndex <= high; loopIndex++) {

			if (index_firstHalf > mid)
				intArray[loopIndex] = aux[index_secondHalf++];
			else if (index_secondHalf > high)
				intArray[loopIndex] = aux[index_firstHalf++];
			else if (aux[index_firstHalf] <= aux[index_secondHalf])
				intArray[loopIndex] = aux[index_firstHalf++];
			else {
				intArray[loopIndex] = aux[index_secondHalf++];
				inversions = inversions + mid - index_firstHalf + 1;
			}
		}
		return inversions;
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
			String[] stringArray = text.trim().split("\\s+");
			//convert to integer array
			int[] intArray = new int[stringArray.length];
			for (int i = 0; i < stringArray.length; i++)
				intArray[i] = Integer.parseInt(stringArray[i]);
			long inversions = countInversions(intArray);
			System.out.println(inversions);

		} catch (FileNotFoundException e) {
			System.out.println("File not found " + new File(args[0]).getAbsolutePath());
		}
	}

}

[/java]
