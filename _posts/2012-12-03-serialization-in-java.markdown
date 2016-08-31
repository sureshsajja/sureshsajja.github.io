---
author: sureshsajja
comments: true
date: 2012-12-03 15:12:11+00:00
layout: post
link: http://coderevisited.com/serialization-in-java/
slug: serialization-in-java
title: Serialization in Java - Part 1
wordpress_id: 191
categories:
- Java
tags:
- java
- Serialization
---

object serialization is the process of encoding objects as byte streams ( known as serializing or deflating or marshalling an object) and reconstructing objects from their byte-stream encoding (known as deserializing or inflating or unmarshalling the object). Once an object has been serialized, its encoding can be stored in a disk, or transmitted over network for later deserialization.
Serializability of a class is enabled by the class implementing the **java.io.Serializable** interface. The Serializable interface in java is a tagging interface type with no methods. Classes **ObjectInputStream** and **ObjectOutputStream** are high-level streams that contain the methods for serializing and deserializing an object.
Let's take a look at one of the simple usages of serialization

    
    import java.io.Serializable;
    
    public class Person implements Serializable {
    
    	private static final long serialVersionUID = 2737304494087343675L;
    
    	private String name;
    
    	public Person(String name) {
    		this.name = name;
    	}
    
    	public String getName() {
    		return name;
    	}
    }



    
    import java.io.FileNotFoundException;
    import java.io.FileOutputStream;
    import java.io.IOException;
    import java.io.ObjectOutputStream;
    
    public class SerializationTest {
    
    	public static void main(String[] args) {
    		Person[] array = new Person[2];
    		array[0] = new Person("Sam");
    		array[1] = new Person("Jack");
    		try {
    			// Open Stream
    			ObjectOutputStream out = new ObjectOutputStream(
    					new FileOutputStream("persons.dat"));
    			// Write to Stream
    			out.writeObject(array);
    			// Close Stream
    			out.close();
    			System.out.println("Serialization of person array is completed");
    		} catch (FileNotFoundException e) {
    			e.printStackTrace();
    		} catch (IOException e) {
    			e.printStackTrace();
    		}
    	}
    }



    
    import java.io.FileInputStream;
    import java.io.FileNotFoundException;
    import java.io.IOException;
    import java.io.ObjectInputStream;
    
    public class DeSerializationTest {
    
    	public static void main(String[] args){
    		try {
    			ObjectInputStream in = new ObjectInputStream(new FileInputStream("persons.dat"));
    			//We know we are reading Person array
    			Person[] array = (Person[]) in.readObject();
    			in.close();
    			System.out.println("Deserialization is completed");
    			System.out.println("Reading person names: "+ array[0].getName() + ", "+array[1].getName());
    		} catch (FileNotFoundException e) {		
    			e.printStackTrace();
    		} catch (IOException e) {		
    			e.printStackTrace();
    		} catch (ClassNotFoundException e) {			
    			e.printStackTrace();
    		}
    	}
    
    }


If Person class doesn't implement Serializable interface and if we try to run SerializationTest, we encounter **java.io.NotSerializableException**. See the output as follows.

    
    java.io.NotSerializableException: Person
    	at java.io.ObjectOutputStream.writeObject0(ObjectOutputStream.java:1164)
    	at java.io.ObjectOutputStream.writeArray(ObjectOutputStream.java:1346)
    	at java.io.ObjectOutputStream.writeObject0(ObjectOutputStream.java:1154)
    	at java.io.ObjectOutputStream.writeObject(ObjectOutputStream.java:330)
    	at SerializationTest.main(SerializationTest.java:17)




### **Points to Note**


All subtypes of a serializable class are themselves serializable. Classes designed for inheritance should rarely implement Serializable, and interfaces should rarely extend it.

**What happens if super classes are not serializable, when subclass is intended for serialization?**
Consider the following example.

    
    public class Animal {
    
    	protected String name;
    
    	public Animal(String name) {
    		this.name = name;
    	}
    
    	public String getName() {
    		return name;
    	}
    
    	public String bark() {
    		return "This is how i bark, better care not";
    	}
    
    }



    
    import java.io.Serializable;
    
    public class Dog extends Animal implements Serializable {
    
    	private static final long serialVersionUID = 7492074643462384609L;
    
    	public Dog(String name) {
    		super(name);
    	}
    
    	public String bark() {
    		return "Bow Bow!!";
    	}
    
    }



    
    import java.io.FileNotFoundException;
    import java.io.FileOutputStream;
    import java.io.IOException;
    import java.io.ObjectOutputStream;
    
    public class SerializableTest {
    
    	public static void main(String[] args) {
    		Dog d = new Dog("Puppy");
    		ObjectOutputStream out = null;
    		try {
    			// Open Stream
    			out = new ObjectOutputStream(new FileOutputStream("dog.dat"));
    			// Write to Stream
    			out.writeObject(d);
    			System.out.println("Serialization of dog Object is completed");
    		} catch (FileNotFoundException e) {
    			e.printStackTrace();
    		} catch (IOException e) {
    			e.printStackTrace();
    		} finally {
    			try {
    				if (out != null)
    					out.close();
    			} catch (IOException e) {
    			}
    
    		}
    	}
    }



    
    import java.io.FileInputStream;
    import java.io.FileNotFoundException;
    import java.io.IOException;
    import java.io.ObjectInputStream;
    
    public class DeSerializableTest {
    
    	public static void main(String args[]) {
    		ObjectInputStream in = null;
    		try {
    			in = new ObjectInputStream(new FileInputStream("dog.dat"));
    			// We know we are reading Person array
    			Dog returnObj = (Dog) in.readObject();
    			in.close();
    			System.out.println("Deserialization is completed");
    			System.out.println("Reading Dog name: " + returnObj.getName());
    		} catch (ClassNotFoundException e) {
    			e.printStackTrace();
    		} catch (FileNotFoundException e) {			
    			e.printStackTrace();
    		} catch (IOException e) {			
    			e.printStackTrace();
    		} finally {
    			try {
    				if (in != null)
    					in.close();
    			} catch (IOException e) {
    			}
    
    		}
    	}
    }


The above code will not work for deserialization, It throws **java.io.InvalidClassException: Dog; no valid constructor**

This is because Animal class is not serializable. To make this code work, we may be tempted to make Animal class implements Serializable but this will not be the case with reality. we may not have access to Animal class.

When super classes are not serializable, subclasses take the responsibility of saving and restoring the state of super-type's public, protected and (if accessible) package fields. For that to happen, super classes should have an accessible no-arg constructor to initialize class's state.

Let's assume, Animal class has default no-arg constructor

    
    public Animal(){
    
    }


will our code work now? no, we still need to work with Dog class to save and restore the state of Animal class.


### Custom Serialized form


**writeObject**
The writeObject method is responsible for writing the state of the object for its particular class so that the corresponding readObject method can restore it. Here is its signature:

    
    private void writeObject(ObjectOutputStream out)throws IOException;


The default mechanism for saving the Object's fields can be invoked by calling out.defaultWriteObject
**readObject**
The readObject method is responsible for reading from the stream and restoring the classes fields.Here is its signature:

    
     private void readObject(ObjectInputStream in) throws IOException, ClassNotFoundException;


It may call in.defaultReadObject to invoke the default mechanism for restoring the object's non-static and non-transient fields.

Adding readObject and writeObject methods to Dog class. Modified Dog class is as follows.

    
    import java.io.IOException;
    import java.io.ObjectInputStream;
    import java.io.ObjectOutputStream;
    import java.io.Serializable;
    
    public class Dog extends Animal implements Serializable {
    
    	private static final long serialVersionUID = 7492074643462384609L;
    
    	public Dog(String name) {
    		super(name);
    	}
    
    	private void writeObject(ObjectOutputStream out) throws IOException {
    		out.defaultWriteObject();
    		out.writeObject(name);
    	}
    
    	private void readObject(ObjectInputStream in) throws IOException,
    			ClassNotFoundException {
    		in.defaultReadObject();
    		this.name = (String) in.readObject();
    	}
    
    	public String bark() {
    		return "Bow Bow!!";
    	}
    
    }


**Transient modifier**
Before i move on, i want to talk about transient modifier, The transient modifier indicates that an instance field is to be omitted from a class’s default serialized form. Transient fields will be initialized to their default values when an instance is deserialized.

**Why it is recommended to call in.defaultReadObject and out.defaultWriteObject in readObject and writeObject methods?**
If all fields are transient, it's technically permissible not to invoke defaultWriteObject and defaultReadObject. But it is recommended. The resulting serialized form makes it possible to add nontransient instance fields in a later release while preserving backward and forward compatibility. If an instance is serialized in a later version and deserialized in an earlier version, the added fields will be ignored. Had the earlier version’s readObject method failed to invoke defaultReadObject, the deserialization would fail with a StreamCorruptedException.

Why should we take the pain, Isn't it simple if we make all classes serializable?
**What are the side effects if classes implement Serializable carelessly?**
Making a class serializable effectively creates a public interface to all fields of that class. Once object is encoded as byte stream, we can decode data in it and can access sensitive data. 
There is a good article on on security guidelines for serialization and deserialization, [read here](http://www.oracle.com/technetwork/java/seccodeguide-139067.html#8).

Concluding part 1 of the Serialization and I will continue in Part 2 on Serializing singleton class, Significance of serialVersionUID. 
