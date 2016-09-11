---
layout: post
title: Serialization in Java - Part 1
description: Serialization in java. Java example for implementing Serializable interface
comments: true
categories:
- Interview
- Java
tags:
- java
- Serialization
author: sureshsajja
---

Object serialization is the process of encoding objects as byte streams 
(known as serializing or deflating or marshalling an object) and reconstructing objects from their byte-stream encoding 
(known as deserializing or inflating or unmarshalling the object).

Once an object has been serialized, its encoding can be stored in a disk, or transmitted over network for later deserialization.
Serializability of a class is enabled by the class implementing the **java.io.Serializable** interface.
The Serializable interface in java is a tagging interface type with no methods.
Classes **ObjectInputStream** and **ObjectOutputStream** are high-level streams that contain the methods for serializing and deserializing an object.
Let's take a look at one of the simple usages of serialization

{% gist 59a119b935b79f43b1744595ee11efc6 %}


If Person class doesn't implement Serializable interface and if we try to run SerializationTest,
we encounter **java.io.NotSerializableException**. See the output as follows.

```java   
java.io.NotSerializableException: Person
    at java.io.ObjectOutputStream.writeObject0(ObjectOutputStream.java:1164)
    at java.io.ObjectOutputStream.writeArray(ObjectOutputStream.java:1346)
    at java.io.ObjectOutputStream.writeObject0(ObjectOutputStream.java:1154)
    at java.io.ObjectOutputStream.writeObject(ObjectOutputStream.java:330)
    at SerializationTest.main(SerializationTest.java:17)
```

### Points to Note

All subtypes of a serializable class are themselves serializable.
Classes designed for inheritance should rarely implement Serializable, and interfaces should rarely extend it.

**What happens if super classes are not serializable, when subclass is intended for serialization?**

Consider the following example.

{% gist 8a191cdcb1d78af94b0dafb316d1d5d3 %}

The above code will not work for deserialization, It throws **java.io.InvalidClassException: Dog; no valid constructor**

This is because Animal class is not serializable. To make this code work, we may be tempted to make Animal class implements Serializable but this will not be the case with reality. we may not have access to Animal class.

When super classes are not serializable, subclasses take the responsibility of saving and restoring the state of super-type's public, protected and (if accessible) package fields. For that to happen, super classes should have an accessible no-arg constructor to initialize class's state.

Let's assume, Animal class has default no-arg constructor

``` java    
public Animal(){
    
}
```

will our code work now? no, we still need to work with Dog class to save and restore the state of Animal class.

### Custom Serialized form

**writeObject**
The writeObject method is responsible for writing the state of the object for its particular class so that the corresponding readObject method can restore it. Here is its signature:

``` java    
private void writeObject(ObjectOutputStream out)throws IOException;
```

The default mechanism for saving the Object's fields can be invoked by calling out.defaultWriteObject
**readObject**
The readObject method is responsible for reading from the stream and restoring the classes fields.Here is its signature:

```java    
private void readObject(ObjectInputStream in) throws IOException, ClassNotFoundException;
```

It may call ```in.defaultReadObject``` to invoke the default mechanism for restoring the object's non-static and non-transient fields.

Adding ```readObject``` and ```writeObject``` methods to Dog class. Modified Dog class is as follows.

{% gist 4a98927764c4c9a3f2ff4ec4ac6b40e8 %}

**Transient modifier**

Before i move on, i want to talk about transient modifier, The transient modifier indicates that an instance field is to be omitted from a class’s default serialized form. Transient fields will be initialized to their default values when an instance is deserialized.

**Why it is recommended to call in.defaultReadObject and out.defaultWriteObject in readObject and writeObject methods?**

 > If all fields are transient, it's technically permissible not to invoke defaultWriteObject and defaultReadObject.
 But it is recommended. The resulting serialized form makes it possible to add nontransient instance fields in a later release while preserving backward and forward compatibility. If an instance is serialized in a later version and deserialized in an earlier version, the added fields will be ignored. Had the earlier version’s readObject method failed to invoke defaultReadObject, the deserialization would fail with a StreamCorruptedException.

Why should we take the pain, Isn't it simple if we make all classes serializable?

**What are the side effects if classes implement Serializable carelessly?**

> Making a class serializable effectively creates a public interface to all fields of that class. Once object is encoded as byte stream, we can decode data in it and can access sensitive data. 

There is a good article on on security guidelines for serialization and deserialization, [read here](http://www.oracle.com/technetwork/java/seccodeguide-139067.html#8).

Concluding part 1 of the Serialization and I will continue in Part 2 on Serializing singleton class, Significance of serialVersionUID. 
