---
layout: post
title: Reverse every k nodes of a linked list
description: Reverse every k nodes of a linked list, Java code for reverse every k nodes of a linked list 
comments: true
categories:
- Algorithms
tags:
- Linked List
- Algorithms
author: sureshsajja
---

## Problem

Write a function to reverse every k nodes of a linked list (k is the input to the function)


### Example:

Given linked list is 1->2->3->4->5->6->7->8>9, 
Input: K = 3  
Output: 3->2->1->6->5->4->9->8->7  
Input: K = 5  
Output: 5->4->3->2->1->9->8->7->6  

## Iterative Solution

{% gist bc9d27620259208d20d7622b5a1bc4bb %}

## Recursive Solution

{% gist f04f4628554448b8b907a3448cf4ff13 %}


Check [here](https://github.com/sureshsajja/CodeRevisited/blob/master/src/com/coderevisited/linkedlists/singly/ReverseKNodesAsGroup.java) for complete code.
