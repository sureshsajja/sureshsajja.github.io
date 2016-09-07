---i
author: sureshsajja
comments: true
date: 2013-12-12 09:52:24+00:00
layout: post
#link: http://coderevisited.com/beginning-jpa-2-0/
slug: beginning-jpa-2-0
title: Beginning with JPA 2.0
wordpress_id: 454
categories:
- Java
- JPA
tags:
- java
- JPA
---

The Java Persistence Architecture API (JPA) is a Java specification for accessing, persisting, and managing data between Java objects / classes and a relational database. JPA allows POJO (Plain Old Java Objects) to be easily persisted. JPA allows an object’s object-relational mappings to be defined through standard annotations or XML defining how the Java class maps to a relational database table.
JPA also defines a runtime EntityManager API for processing queries and transaction on the objects against the database. JPA defines an object-level query language, JPQL, to allow querying of the objects from the database.





## Why JPA




It is a standard for the management of persistence and object/relational mapping with Java EE and Java SE. JPA supports the large data sets, data consistency, concurrent use, and query capabilities of JDBC. Like object-relational software and object databases, JPA allows the use of advanced object-oriented concepts such as inheritance. JPA avoids vendor lock-in by relying on a strict specification. JPA focuses on relational databases. And JPA is extremely easy to use.
Currently most of the persistence vendors have released implementations of JPA confirming its adoption by the industry and users. These include Implementations include Hibernate, TopLink, and Kodo JDO, Cocobase and JPOX.
Java Persistence consists of four areas






	
  * The Java Persistence API

	
  * The query language

	
  * The Java Persistence Criteria API

	
  * Object/relational mapping metadata




## Entities




In Object oriented paradigm, An entity is a lightweight persistence domain object. Typically, an entity represents a table in a relational database, and each entity instance corresponds to a row in that table. Changing values of entity instance works just like changing values on any other class instance. The difference is that you can persist those changes in database.
The primary programming artifact of an entity is the entity class, although entities can use helper classes. The persistent state of an entity is represented through either persistent fields or persistent properties. These fields or properties use object/relational mapping annotations to map the entities and entity relationships to the relational data in the underlying data store.





## Characteristics of an Entity





	
  1. Persistability

	
    * 


An Entity is persistable since it can be saved to a persistence store. But this doesn’t happen automatically. It has to be invoked by API. It is important because it leaves control over persistence to application.







	
  2. Identity

	
    * 


Identifier, is the key that uniquely identifies an entity instance and distinguishes it from all the other instances of the same entity type.







	
  3. Transactionality

	
    * 


Changes made to the database either succeed or fail atomically, so the persistent view of an entity should indeed be transactional.







	
  4. Granularity

	
    * 


Entities are meant to be fine-grained objects and they are business domain objects that have specific meaning to the application that accesses them.










## Example Entity class with annotations metadata



    
    import javax.persistence.*;
    
    @Entity
    @Table(name = "Employee")
    public class Employee {
    
        @Id
        @Column(name = "id")
        private int id;
    
        @Column(name = "fistName")
        private String firstName;
    
        @Column(name = "lastName")
        private String lastName;
    
        @Column(name = "dept")
        private String dept;
    
        public Employee(){
    
        }
    
        public Employee(int id, String firstName, String lastName, String dept){
            this.setId(id);
            this.setFirstName(firstName);
            this.setLastName(lastName);
            this.setDept(dept);
        }
    
        public int getId() {
            return id;
        }
    
        public void setId(int id) {
            this.id = id;
        }
    
        public String getFirstName() {
            return firstName;
        }
    
        public void setFirstName(String firstName) {
            this.firstName = firstName;
        }
    
        public String getLastName() {
            return lastName;
        }
    
        public void setLastName(String lastName) {
            this.lastName = lastName;
        }
    
        public String getDept() {
            return dept;
        }
    
        public void setDept(String dept) {
            this.dept = dept;
        }
    }




## Managing Entities




Entities are managed by the entity manager. Each EntityManager instance is  associated with a persistence context: a set of managed entity instances that exist in a particular data store. A persistence context defines the scope under which particular entity instances are created, persisted, and removed. The EntityManager interface defines the methods that are used to interact with the persistence context.




EntityManager is to be implemented by a particular persistence provider. It is the provider that supplies the backing implementation engine for the entire Java Persistence API, from the EntityManager through to implementation of the query classes and SQL generation.








## Illustration of Entity manager



    
    import javax.persistence.EntityManager;
    import javax.persistence.EntityManagerFactory;
    import javax.persistence.Persistence;
    
    public class EmployeeTest {
    
        private static EntityManager em;
    
        public static void main(String[] args) {
    
            EntityManagerFactory emf = Persistence.createEntityManagerFactory("EmployeeService");
            em = emf.createEntityManager();
    
            createEmployee(1, "Saint", "Peter", "Engineering");
            createEmployee(2, "Jack", " Dorsey", "Imaginea");
            createEmployee(3, "Sam", "Fox", "Imaginea");
    
        }
    
        private static void createEmployee(int id, String firstName, String lastName, String dept) {
            em.getTransaction().begin();
            Employee emp = new Employee(id, firstName, lastName, dept);
            em.persist(emp);
            em.getTransaction().commit();
        }
    }




## Project setup





	
  * Create a  simple java project with maven [http://maven.apache.org/plugins-archives/maven-archetype-plugin-1.0-alpha-7/examples/simple.html](http://maven.apache.org/plugins-archives/maven-archetype-plugin-1.0-alpha-7/examples/simple.html)

	
  * Maven dependencies



    
    <dependencies>
            <dependency>
                <groupId>org.eclipse.persistence</groupId>
                <artifactId>javax.persistence</artifactId>
                <version>2.0.0</version>
            </dependency>
    
            <dependency>
                <groupId>org.hibernate</groupId>
                <artifactId>hibernate-entitymanager</artifactId>
                <version>4.2.8.Final</version>
            </dependency>
    
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>5.1.27</version>
            </dependency>
        </dependencies>





	
  * Setup mysql and use the following sql code to create "Employee" table in test database



    
    use test;
    
    create table Employee (
           id INT NOT NULL,
           fistName VARCHAR(20) default NULL,
           lastName VARCHAR(20) default NULL,
           dept VARCHAR(20) default NULL,
           PRIMARY KEY (id)
    );





	
  * Create META-INF/persistence.xml file in resources directory. It specifies JPA provider and it's properties.



    
    <?xml version="1.0" encoding="UTF-8"?>
    <persistence xmlns="http://java.sun.com/xml/ns/persistence"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:schemaLocation="http://java.sun.com/xml/ns/persistence
                 http://java.sun.com/xml/ns/persistence/persistence_2_0.xsd"
                 version="2.0">
    
        <persistence-unit name="EmployeeService">
            <provider>org.hibernate.ejb.HibernatePersistence</provider>
            <properties>
                <property name="hibernate.connection.url" value="jdbc:mysql://localhost/test"/>
                <property name="hibernate.connection.driver_class" value="com.mysql.jdbc.Driver"/>
                <property name="hibernate.connection.username" value="root"/>
                <property name="hibernate.connection.password" value="password"/>
                <property name="hibernate.archive.autodetection" value="class"/>
                <property name="hibernate.show_sql" value="true"/>
                <property name="hibernate.format_sql" value="true"/>
                <property name="hbm2ddl.auto" value="update"/>
            </properties>
        </persistence-unit>
    </persistence>





	
  * Add Employee.java, EmployeeTest.java files to project

	
  * You should be able to execute EmployeeTest which will insert 3 records to Employee table.


The sample code can be downloaded from [here](https://github.com/sureshsajja/JPA101)
