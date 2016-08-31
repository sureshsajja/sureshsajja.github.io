---
author: sureshsajja
comments: true
date: 2012-11-30 02:01:17+00:00
layout: post
link: http://coderevisited.com/cloneable-interface-in-java/
slug: cloneable-interface-in-java
title: Cloneable Interface in Java
wordpress_id: 152
categories:
- Java
tags:
- Cloneable
- java
---

This post will give some deep insight into Cloneable Interface in Java.

A  clone of an object is an object with distinct identity and equal contents. To define clone, a class must implement cloneable interface and must override Object's clone method with a public modifier. At this point, it is worth nothing that cloneable interface does not contain the clone method, and Object’s clone method is protected.

When the Object class finds that the object to be cloned isn't an instance of a class that implements cloneable interface, it throws **CloneNotSupportedException.**

    
    public class Person {
    
    	private String name;
    
    	public Person(String name) {
    		this.name = name;
    	}
    
    	public String getName() {
    		return name;
    	}
    
    	public static void main(String[] args) {
    		Person p = new Person("Sam");
    		try {
    			// Person class doesn't implement Cloneable but tries to get clone. It fails
    			// with CloneNotSupportedException
    			Person pClone = (Person) p.clone();
    			System.out.println(pClone.getName());
    		} catch (CloneNotSupportedException e) {
    			e.printStackTrace();
    		}
    	}
    
    }


If a class wants to allow clients to clone it's instances, it must override **Object's clone method with a public modifier.**

    
    public class Person implements Cloneable{
    
    	private String name;
    
    	public Person(String name) {
    		this.name = name;
    	}
    
    	public String getName() {
    		return name;
    	}
    
    	public Person clone() {
    		try {
    			return (Person) super.clone();
    		} catch (CloneNotSupportedException e) {		
    			e.printStackTrace();
    			throw new RuntimeException();
    		}
    	}
    }



    
    public class CloneableTest {
    
    	public static void main(String[] args) {
    		Person p = new Person("Sam");
    		Person pClone = p.clone(); // Can you do this, if clone method in Person class is not public
    		System.out.println(pClone.getName());
    	}
    }




Now let's look into the details of** how to implement clone method.**

If you override the clone method in a non final class, you should return an object obtained by invoking **super.clone**. If all of a class’s super classes obey this rule, then invoking super.clone will eventually invoke Object’s clone method, creating an instance of the right class.

A small example, if classes don't obey this rule.

    
    public class Animal implements Cloneable {
    
    	private String name;
    
    	public Animal(String name) {
    		this.name = name;
    	}
    
    	public String getName() {
    		return name;
    	}
    
    	public String bark() {
    		return "This is how i bark, better care not";
    	}
    
    	public Animal clone() {
    		// violation of contract to call super.clone() when creating instance of
    		// the right class
    		return new Animal(name);
    	}
    
    }



    
    public class Dog extends Animal {
    
    	public Dog(String name) {
    		super(name);
    	}
    
    	public String bark() {
    		return "Bow Bow!!";
    	}
    
    	public Dog clone() {
    		return (Dog) super.clone();
    	}
    
    }



    
    public class CloneableTest {
    
    	public static void main(String[] args) {
    		Dog dog = new Dog("Puppy");
    		Dog dogClone = dog.clone(); //Do you think, you can do this?
    		System.out.println(dogClone.getName());
    
    	}
    }



    
    Exception in thread "main" java.lang.ClassCastException: Animal cannot be cast to Dog
    	at Dog.clone(Dog.java:12)
    	at CloneableTest.main(CloneableTest.java:5)




Once you get hold of object from super.clone(), we may have to do some modifications depending on nature of the class
If every field contains a primitive value or a reference to an immutable object, no further processing is necessary. If there are references to mutable objects, in order to work clone properly, it is required to call clone on those mutable references.

Example **to clone mutable ****references**

    
    import java.util.Date;
    
    public class Person implements Cloneable {
    
    	private String name;
    	private Date dob;
    
    	public Person(String name, Date dob) {
    		this.name = name;
    		this.dob = dob;
    	}
    
    	public String getName() {
    		return name;
    	}
    
    	public Person clone() {
    		Person p;
    		try {
    			p = (Person) super.clone();
    			p.dob = (Date) dob.clone();
    			return p;
    		} catch (CloneNotSupportedException e) {
                            e.printStackTrace();
    			throw new RuntimeException();
    		}
    
    	}
    }


If there are  final fields referring to mutable objects,  In order to make a class cloneable, it may be necessary to remove final modifiers from some fields.
If you decide to make a thread-safe class implement Cloneable, remember that its clone method must be properly synchronized just like any other method

**Final Note:**
Implementation of clone is very tricky. Avoid implementing it and look for other alternatives.
