---
layout: post
title: How to convert integer to byte array in java
description: how to convert integer to byte array in java and to convert byte array to integer
comments: true
categories:
- Algorithms
- Interview
tags:
- Algorithms
- byte array
author: sureshsajja
---

This post demonstrates how to convert integer to byte array in java and to convert byte array to integer.
It shows low level and high level byte operations.

* In java, integer takes four bytes. So it is assumed that byte array is of length 4.
* Byte order can be LITTLE_ENDIAN or BIG_ENDIAN

A short note on endianness:
For LITTLE_ENDIAN : Least significant bit is at lowest memory address
For BIG_ENDIAN : Most significant bit is at lowest memory address


{% gist fe5437b0b287af6dd402043801bd5fb9 %}

 




