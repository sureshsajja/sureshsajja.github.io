---
layout: post
title: Insertion Sort
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
**Best Case:** Array is already sorted. So no swapping of elements are required. It required \\( n-1 \\) comparisons (condition of inner loop, body never executed)  

**Worst Case:** Array is sorted in reverse order. (So each item has to be moved to the front of the array)  
No of comparisons: \\( 0+1+2+3+......+n-1 = (n-1)(n-2)/2 \\)  
No of swaps: \\( n(n-1)/2 \\)

**Average Case**: For randomly ordered arrays  
No of comparisons: \\( ~n(n-1)/4 \\)  
No of swaps: \\( ~n(n-1)/4 \\)

### Example Code in Java

{%gist 2ab1436805d84dcac402918e1b2e0c00 %}

### Variations of insertion sort algorithm

Notice that we have a double test for the inner loop.  
 
```java
    j > 0 && (array[j] < array[j - 1]
```


The first condition is to ensure that we don’t run off the beginning of the array, and the second one is to stop the loop once we reach the correct spot to insert the item. we can avoid an index-out-of-bounds test in the inner loop by first putting the smallest item into position. The item that enables the test to be eliminated is known as a sentinel.

### Example Code for sort with sentinel
 
{%gist c686ec92c1031ed407717d40aadb6f1a %}


### Questions
* Which sort algorithm runs fastest for an array with all keys identical, selection sort or insertion sort?  

 > Insertion sort runs in linear time when all keys are equal.

* Suppose that we use insertion sort on a randomly ordered array where items have only one of three key values. Is the running time linear, quadratic, or something in between?  
    
 > Quadratic
