---
author: sureshsajja
comments: true
date: 2013-12-18 06:58:09+00:00
layout: post
#link: http://coderevisited.com/beginng-with-spring-data-jpa/
slug: beginng-with-spring-data-jpa
title: 'Beginning with Spring Data JPA '
wordpress_id: 467
categories:
- Java
---

In my [previous post](http://coderevisited.com/beginning-jpa-2-0/), I briefly wrote about JPA, a Java specification for accessing, persisting, and managing data between Java objects / classes and a relational database. In this post, I will talk about Spring Data JPA with a simple example.





## Why Spring Data JPA?




The data access code which uses the Java Persistence API contains a lot of unnecessary boilerplate code. Too much boilerplate code has to be written  If we have to write dynamic queries or implement pagination.




Spring Data JPA is another layer of abstraction for the support of persistence layer in spring context. The goal of Spring Data repository abstraction is to significantly reduce the amount of boilerplate code required to implement data access layers for various persistence stores. We just have to write repository interfaces, including custom finder methods, and Spring will provide the implementation automatically.





## Defining repository interfaces




As a first step we define a domain class-specific repository interface. The interface must extend Repository and be typed to the domain class and an ID type. If you want to expose CRUD methods for that domain type, extend CrudRepository instead of Repository. The CrudRepository provides sophisticated CRUD functionality for the entity class that is being managed.  We can add additional query methods to this repository interface.




    
    public interface EmployeeRepository extends CrudRepository<Employee, Integer> {
    }


Note: Employee entity class is defined in [previous post](http://coderevisited.com/beginning-jpa-2-0/).


## XML configuration


Each Spring Data module includes repositories element that allows you to simply define a base package that Spring scans for you.  Spring bean configurations are used to configure JPA vendor, data source for creating entity manager.

    
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:jpa="http://www.springframework.org/schema/data/jpa"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/data/jpa
        http://www.springframework.org/schema/data/jpa/spring-jpa.xsd">
    
        <jpa:repositories base-package="com.coderevisited.spring"/>
    
        <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
            <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost/test"/>
            <property name="username" value="root"/>
            <property name="password" value="password"/>
        </bean>
    
        <bean id="jpaVendorAdapter" class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
            <property name="showSql" value="true"/>
            <property name="generateDdl" value="true"/>
            <property name="database" value="MYSQL"/>
        </bean>
    
        <bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
            <property name="dataSource" ref="dataSource"/>
            <property name="jpaVendorAdapter" ref="jpaVendorAdapter"/>
            <!-- spring based scanning for entity classes-->
            <property name="packagesToScan" value="com.coderevisited.spring"/>
        </bean>
    
        <bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager"/>
    
    </beans>




## Querying Repository


We can execute query methods from application class by referring to repository bean.

    
    public class EmployeeTest {
    
        private static CrudRepository repository;
    
        public static void main(String[] args) {
            AbstractApplicationContext context = new ClassPathXmlApplicationContext("spring-config.xml");
            repository = context.getBean(EmployeeRepository.class);
    
            createEmployee(22, "Saint", "Peter", "Engineering");
            createEmployee(23, "Jack", " Dorsey", "Imaginea");
            createEmployee(24, "Sam", "Fox", "Imaginea");
    
            context.close();
    
        }
    
        private static void createEmployee(int id, String firstName, String lastName, String dept) {
    
            Employee emp = new Employee(id, firstName, lastName, dept);
            repository.save(emp);
        }
    
    }


The above application code when executed would insert 3 rows to Employee table in test database.

The code for this example can be downloaded from [here](https://github.com/sureshsajja/SpringDataJPA)
