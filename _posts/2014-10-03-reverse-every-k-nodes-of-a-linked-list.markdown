---
author: sureshsajja
comments: true
date: 2014-10-03 20:40:05+00:00
layout: post
slug: reverse-every-k-nodes-of-a-linked-list
title: Reverse every k nodes of a linked list
description: Function to reverse every k nodes of a linked list
categories:
- Java
image:
  feature: abstract-1.jpg
---

## Problem
Write a function to reverse every k nodes of a linked list (k is the input to the function)


### Example:
Given linked list is 1->2->3->4->5->6->7->8>9  
Input: K = 3  
Output: 3->2->1->6->5->4->9->8->7  
Input: K = 5  
Output: 5->4->3->2->1->9->8->7->6  


## Iterative Solution

```
public static SinglyLinkedListNode reverseKNodes(SinglyLinkedListNode head, int k)
    {
        //Start with head
        SinglyLinkedListNode current = head;
        //last node after reverse
        SinglyLinkedListNode prevTail = null;
        //first node before reverse
        SinglyLinkedListNode prevCurrent = head;
        while (current != null) {

            //loop for reversing K nodes
            int count = k;
            SinglyLinkedListNode tail = null;
            while (current != null && count > 0) {
                SinglyLinkedListNode next = current.getNext();
                current.setNext(tail);
                tail = current;
                current = next;
                count--;
            }
            //reversed K Nodes

            if (prevTail != null) {
                //Link this set and previous set
                prevTail.setNext(tail);
            } else {
                //We just reversed first set of K nodes, update head point to the Kth Node
                head = tail;
            }
            //save the last node after reverse since we need to connect to the next set.
            prevTail = prevCurrent;
            //Save the current node, which will become the last node after reverse
            prevCurrent = current;
        }

        return head;
    }

```
 
## Recursive Solution

```
public static SinglyLinkedListNode reverseKNodesRecursive(SinglyLinkedListNode head, int k)
    {
        SinglyLinkedListNode current = head;
        SinglyLinkedListNode next = null;
        SinglyLinkedListNode prev = null;
        int count = k;

        //Reverse K nodes
        while (current != null && count > 0) {
            next = current.getNext();
            current.setNext(prev);
            prev = current;
            current = next;
            count--;
        }

        //Now next points to K+1 th node, returns the pointer to the head node
        if (next != null) {
            head.setNext(reverseKNodesRecursive(next, k));
        }
        //return head node
        return prev;
    }
```
 


Check [here](https://github.com/sureshsajja/CodeRevisited/blob/master/src/com/coderevisited/linkedlists/singly/ReverseKNodesAsGroup.java) for complete code.
