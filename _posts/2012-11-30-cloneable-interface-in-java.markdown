---
layout: post
title: Cloneable Interface in Java
description: Cloneable interface in java. Java example for implementing Cloneable interface
comments: true
categories:
- Interview
- Java
tags:
- java
- Cloneable
author: sureshsajja
---

This post will give some deep insight into Cloneable Interface in Java.

A  clone of an object is an object with distinct identity and equal contents.
To define clone, a class must implement cloneable interface and must override Object's clone method with a public modifier.
At this point, it is worth nothing that cloneable interface does not contain the clone method, and Object’s clone method is protected.

When the Object class finds that the object to be cloned isn't an instance of a class that implements cloneable interface,
it throws **CloneNotSupportedException.**

{% gist 57008e39ada4475194aa8df56ae297d9 %}


If a class wants to allow clients to clone it's instances, it must override **Object's clone method with a public modifier.**

{% gist 05b58353bc507c826daba15cfb1a4451 %}


Now let's look into the details of **how to implement clone method.**

If you override the clone method in a non final class, you should return an object obtained by invoking **super.clone**. If all of a class’s super classes obey this rule, then invoking super.clone will eventually invoke Object’s clone method, creating an instance of the right class.

A small example, if classes don't obey this rule.

{% gist 24e216cd4ba8cd9953dd8ba60b2f2aa7 %}


Once you get hold of object from super.clone(), we may have to do some modifications depending on nature of the class
If every field contains a primitive value or a reference to an immutable object, no further processing is necessary.
If there are references to mutable objects, in order to work clone properly, it is required to call clone on those mutable references.

Example **to clone mutable references**

{% gist 2f606eb69a51376e3fb3e2238db6387c %}


If there are  final fields referring to mutable objects,  In order to make a class cloneable, it may be necessary to remove final modifiers from some fields.
If you decide to make a thread-safe class implement Cloneable, remember that its clone method must be properly synchronized just like any other method

**Final Note:**
Implementation of clone is very tricky. Avoid implementing it and look for other alternatives.