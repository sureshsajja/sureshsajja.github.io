---
layout: post
title: Infix expression to Postfix expression
description: Infix expression to Postfix expression, Java code for Infix expression to Postfix expression, postfix evaluation, Java code for postfix evaluation
comments: true
categories:
- Java
- Algorithm
tags:
- Java
- Algorithm
author: sureshsajja
---

This post shows sample implementations for
1. Converting an infix expression to postfix expression (Reverse polish notation)
2. Evaluating postfix expression

### Infix to PostFix notation

To convert from infix to postfix notation, I have implemented Dijkstra's shunting-yard algorithm.
For detailed algorithm, please take a look at [wiki page](http://en.wikipedia.org/wiki/Shunting-yard_algorithm).

### Evaluating postfix expression

This program evaluates postfix expresions using a stack.
The following is sample implementation in java
 

### Sample code for Infix to PostFix

{% gist 8c6ae0275f9644daae052585777cee6c %}
    

### Sample code for PostFix evaluation

{% gist 446f033185b1639ed6bff242170136b8 %}

Check [here](https://github.com/sureshsajja/CodingProblems/tree/master/src/main/java/com/coderevisited/stack) for complete code.

For other libraries used in the above program, please refer to GitHub repo [here](https://github.com/sureshsajja)
