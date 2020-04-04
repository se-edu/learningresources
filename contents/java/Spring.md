<frontmatter>
  title: Introduction to Spring Framework
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Introduction to Spring Framework

**Authors: [Liu Yiwen](https://github.com/0blivious)** <br>
Reviewers: []()

<box type="info">
This chapter assumes that the reader has a basic knowledge of Java enterprise.
</box>

<box id="article-toc">

* [What is Spring Framework‎](#what-is-spring-framework)
* [Core Features](#core-features)
  * [The IOC Container](#the-ioc-container)
  * [Spring JdbcTemplate](#spring-jdbctemplate)
  * [Spring AOP](#spring-aop)
* [Why use Spring‎](#why-use-spring)
  * [Benefit 1: Modularity](#benefit-1-modularity)
  * [Benefit 2: Easy Integration](#benefit-2-easy-integration)
  * [Benefit 3: Strong Community Support](#benefit-3-strong-community-support)
  * [Benefit 4: Usability](#benefit-4-usability)
* [How to get started‎](#how-to-get-started)
</box>

## What is Spring Framework?

The [official website](https://docs.spring.io/spring/docs/current/spring-framework-reference/overview.html) describes Spring Framework as follows:

>*Spring Framework* is an open-source framework to create Java enterprise applications.
>It provides foundational support for different application architectures, including messaging, transactional data and persistence, and web.

## Core Features

### The IoC Container

Inversion of Control (IoC) is a design principle. As the name suggests, it is used to invert
different kinds of controls in object-oriented design to achieve looser coupling.
Here, controls refer to any additional responsibilities a class has,
other than its main responsibility. For example, this may include the control over the flow of an application,
and control over the flow of an object creation.

The Spring IoC container is at the core of the Spring Framework. The container will create the
<tooltip content="These objects are are called beans in Spring Framework. A bean is an object that is instantiated,
assembled, and otherwise managed by a Spring IoC container.">
<i>objects</i>
</tooltip> 
, wire them together,
configure them, and manage their complete life cycle from creation till destruction. The Spring container uses
<tooltip content="Dependency injection is a way of providing a class with the required services.">
<i>dependency injection</i>
</tooltip> to manage the components that make up an application.
    
In the Spring framework, the IoC container is represented by the interface `ApplicationContext`.
The beans are created with the configuration metadata that you supply to the container,
which can be in the form of XML configuration or annotations.

Below is one way to manually instantiate a container:
```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
```

For example, say we have an Employee class like so:
```java
package com.company;  
  
public class Employee {  
    private int id;  
    private String name;  
    private String city;  
  
    public int getId() {  
        return id;  
    }  
    public void setId(int id) {  
        this.id = id;  
    }  
    public String getName() {  
        return name;  
    }  
    public void setName(String name) {  
        this.name = name;  
    }  
    void display(){  
        System.out.println(id+" "+name);  
    }
}        
```

We can then populate the Employee bean with the data provided in the following `applicationContext.xml` file:
This makes our code loosely coupled and easier for testing. 
```xml
<bean id="obj" class="com.company.Employee">  
    <property name="id">  
        <value>20</value>  
    </property>  
    <property name="name">  
        <value>Amy</value>  
    </property>  
</bean>  
```
   
To better understand how this dependency injection work, we can have a `Test.java` looks like the following:
```java
Resource r = new ClassPathResource("applicationContext.xml");  
BeanFactory factory = new XmlBeanFactory(r);  
          
Employee e = (Employee)factory.getBean("obj");  
e.display();
```

The output of the above code would then be `20 Amy`.

### Spring JdbcTemplate

The Spring JdbcTemplate is a powerful mechanism to connect to the database and execute SQL queries. Traditionally, we
need to use Java Database Connectivity (JDBC) API to access tabular data stored in any relational database.
It is a part of JavaSE (Java Standard Edition), from Oracle Corporation.
    
In a traditional JDBC API:
- a typical SQL query would involve quite some amount of boilerplate code, such as creating connection,
statement, closing resultset, connection etc.
- exception handling has to be manually written out.
- transactions need to be manually created.

The Spring JdbcTemplate provides methods to write the queries directly, hence, saving time in writing such boilerplate code.

Task | Spring | You
-----|--------|----
Connection Management | :heavy_check_mark: | 
Writing SQL Queries | | :heavy_check_mark: 
Statement Management | :heavy_check_mark: |
ResultSet Management | :heavy_check_mark: |
Row Data Retrieval | | :heavy_check_mark: 
Parameter Declaration | | :heavy_check_mark: 
Parameter Setting | :heavy_check_mark: |
Transaction Management | :heavy_check_mark: |
Exception Handling | :heavy_check_mark: |

### Spring AOP

Aspect Oriented Programming (AOP) is a programming paradigm that aims to increase modularity by allowing the separation of
<tooltip content="Concerns that can affect the whole application and should be centralized in one location in code as much as possible. Examples of these concerns include transaction management, authentication, logging, security etc.">
<i>cross-cutting concerns</i></tooltip>.
It does so by adding additional behavior to existing code
without modifying the code itself, instead separately specifying which code is modified via a "pointcut"
specification. This allows behaviors that are not central to the business logic (such as logging) to be added to a program
without cluttering the core code. AOP forms a basis for aspect-oriented software development.

Spring Framework supports AOP and enables cohesive development, by providing ways to dynamically add the
cross-cutting concern before, after or around the actual logic using simple
pluggable configurations. 

For example, say we wish to log something into the console everytime before we call the method `getEmployeeById`.
We can then write aspect class annotated with `@Aspect` annotation and write point-cut expressions to log program information from joint points
<tooltip content="Join point is a point of execution of the program, such as the execution of a method or the handling of an exception.">
<i>joint-point</i>
</tooltip> methods.
```java
@Aspect
public class EmployeeCRUDAspect {
      
    @Before("execution(* EmployeeManager.getEmployeeById(..))") // point-cut expressions
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("EmployeeCRUDAspect.logBefore() : "
                + joinPoint.getSignature().getName());
    }
    
    @After("execution(* EmployeeManager.getEmployeeById(..))") // point-cut expressions
    public void logAfter(JoinPoint joinPoint) {
        System.out.println("EmployeeCRUDAspect.logAfter() : "
                + joinPoint.getSignature().getName());
    }
}
```
    
And the corresponding `EmployeeManager.java` file could be:
```java
@Component
public class EmployeeManager
{
    public EmployeeDTO getEmployeeById(Integer employeeId) {
        System.out.println("Method getEmployeeById() called");
        return new EmployeeDTO();
    }
}
```
In the above example, `logBefore()` will be executed before the `getEmployeeById()` method because it matches the join point specified earlier.
Similarly, `logAfter()` will be executed after `getEmployeeById()`.
The out put of the above code will be:
```
EmployeeCRUDAspect.logBefore() : getEmployeeById
Method getEmployeeById() called
EmployeeCRUDAspect.logAfter() : getEmployeeById
```

The declaration of aspects and
<tooltip content="Advice is an action taken by an aspect at a particular join point.">
<i>advices </i>
</tooltip>could also be written directly in the XML templates. This makes the code more readable and
also allows for easier maintainance as we can add/remove concerns 
by changing XML configuration file without recompiling source code.

For example, the above configuration could be written in a XML file as:
```xml
 <aop:config>
    <!-- Spring AOP Pointcut definitions -->
    <aop:pointcut id="CRUDOperation" expression="execution(* EmployeeManager.*(..))" />
 
    <!-- Spring AOP aspect 1 -->
    <aop:aspect id="EmployeeCRUD" ref="crudAspectBean">
             
        <!-- Spring AOP advises -->
        <aop:before pointcut-ref="CRUDOperation" method="logBefore" />
        <aop:after pointcut-ref="CRUDOperation" method="logAfter" />
             
    </aop:aspect>

    <!-- Spring AOP aspect instances -->
    <bean id="crudAspectBean" class="com.aop.EmployeeCRUDAspect" />
     
    <!-- Target Object -->
    <bean id="employeeManager" class="com.aop.EmployeeManagerImpl" />
</aop:config>
```

## Why Use Spring?

Spring makes programming Java quicker, easier, and safer for everybody.
Similar to other general Java frameworks (e.g. [Grails](https://grails.org/), [Play](https://www.playframework.com/)), Spring helps us focus on the core task rather than the boilerplate associated with it.
Apart from that, Spring has other advantages like:

### Benefit 1: Modularity

Spring provides different modules to achieve different services and functionality for development of application.
These modules are designed in such a way that no module is dependent on the others, except the Spring core module.
Thus, we can optionally include one or more Spring projects depending on the need. This makes Spring lightweight
and require less configuration.

![Spring Framework Overview](spring-overview.png "Overview of the Spring Framework")

_Figure 1. Overview of the Spring Framework_

### Benefit 2: Easy Integration
Spring is designed to be used compatible with many other frameworks of Java, for example
<tooltip content="Struts is an open source framework that extends the Java Servlet API and employs a Model, View, Controller (MVC) architecture.">
<i>Struts </i>
</tooltip>and
<tooltip content="Hibernate is an object-relational mapping tool for the Java programming language. It provides a framework for mapping an object-oriented domain model to a relational database.">
<i>Hibernate</i>
</tooltip>. Spring framework do not impose any restriction on the frameworks to be used together.

### Benefit 3: Strong Community Support
Spring is an open source framework led by [Pivotal](https://tanzu.vmware.com/) Software and backed by a large consortium of organizations and individual
developers. This means that it remains relevant,
as evident by the ever increasing number of projects under its umbrella, which is ever increasing. 

### Benefit 4: Usability
One of the key aspects of any framework's popularity is how easy it is for developers to use it.
Projects like [Spring Boot](https://spring.io/projects/spring-boot) have made bootstrapping a complex Spring project almost trivial.
Spring thus is very easy to start but also powerful to extend to what programmers exactly need by providing multiple configuration options.
Not to mention, it has excellent documentation and tutorials to help anyone get on-boarded.

## Getting Started

The official website is a great place to get started. It includes:
- [Spring Quickstart Guide](https://spring.io/quickstart)
- [Spring Projects Overview](https://spring.io/projects)

Here are some external resources that can be useful:
- [Baeldung](https://www.baeldung.com/)
- [Spring Tutorial](https://www.tutorialspoint.com/spring/index.htm)

If you need help with Spring, you can get support from Spring's community of millions of developers:
- [Spring Community](https://spring.io/community)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/spring)
- [Gitter](https://gitter.im/spring-projects/home)

</div>
