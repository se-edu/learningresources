# Java 8 Streams - An Introduction

Author: Lee Yi Min


<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Overview](#overview)
- [Getting Started](#getting-started)
	- [Functional Interface and Lambda Expressions](#functional-interface-and-lambda-expressions)
		- [Functional Interface](#functional-interface)
		- [Method Reference](#method-reference)
		- [Lambda Expressions](#lambda-expressions)
		- [An Example](#an-example)
	- [What is a stream?](#what-is-a-stream)
- [Using Java 8 Streams](#using-java-8-streams)
	- [Drawbacks and Pitfalls](#drawbacks-and-pitfalls)
		- [Long, complicated lambda expressions](#long-complicated-lambda-expressions)
- [Resources](#resources)
	- [Functional Interfaces](#functional-interfaces)
	- [Method References](#method-references)
	- [Lambda Expressions](#lambda-expressions)
	- [Stream](#stream)

<!-- /TOC -->
## Overview

In Java 8, we were introduced to new features such as lambda expressions and streams. If you weren't familiar with the concept of functional programming, you might be silently screaming in your head as you stare at a chunk of code infused with lambda expressions and stream operations.

However, oftentimes, it's hard not to admire the conciseness of the codes utilising these features. For example, you might want to calculate the mean height of male students given a list of students. Using traditional for-loops, you may write something like this:

```java
double totalMaleHeight = 0;
int noOfMales = 0;
for (Student student: students) {
  if (student.isMale()) {
    totalMaleHeight += student.getHeight();
    noOfMales++;
  }
}
double meanMaleHeight = totalMaleHeight / noOfMales;
```

However, with the power of the Stream API, you can write this instead!

```java
double meanMaleHeight = students.toStream()
                                .filter(student -> student.isMale())
                                .mapToDouble(student -> student.getHeight())
                                .average()
                                .orElse(Double.NaN);
```

Not only is the numbers of lines reduced by almost half, the code utilising streams is also rather intuitive. First, we filter the students who are male, using a lambda expression (`student -> student.isMale()`). Then, we get the heights of these students and followed by the average value of these heights. If the average value does not exist (which can happen when there are no male students), we store NaN in `meanMaleHeight` instead. The code is declarative and self-documentating, it's easy to understand what the original author was trying to achieve. This reduces the need for code comments, which we often see in loops since they can be harder to understand at a glance.

Besides its brevity, another cool feature of streams is that code can be executed in parallel using your multicore processor. Multiple students can be processed simultaneously, compared to processing only one student at a time with a normal loop. This can help to improve the performance of the operation significantly. And the best part is this can be done just by adding a simple method call in the Stream API, so you don't have implement multithreading or worry about how to go about splitting the work for it to work in parallel.

It's okay if you have no experience in writing lambda expressions! This guide will step you through the basics of both lambda expressions and streams so that you can start utilising them.

## Getting Started

Before we can get started on Stream, it is imperative to first have some understanding on the concept of functional interface and lambda expressions. Feel free to skip right ahead if you are already familiar with such concepts! Then, let's take a closer look at the Stream API to get a better understanding of how it works.

### Functional Interface and Lambda Expressions

#### Functional Interface
If you have tried using the Stream library and looked at its [API page](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html), you would see that many of its methods accept objects of new types such as `Predicate`, `Collector` and more. If you navigate into the APIs of these new types, you would have realised they are merely interfaces and the APIs themselves aren't giving much help on how you are supposed to make use of these interfaces.

These interfaces are actually functional interfaces, which are simply __interfaces declared with a single abstract method__.

As these interfaces only have a single abstract method, when you provide a method reference or a lambda expression where a functional interface is required, the compiler is able to self-infer and instantiate an object of the required functional interface type, with the given method/lambda as the implementation of the abstract method. This is similar to instantiating an [anonymous class object](https://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html), except that the code is more concise with the new syntax.

#### Method Reference

So how can you provide a method reference? The syntax is simple : `<class/object/interface>::<method name>`.  
The method could be
1. a static method of a class (`MyClass::staticMethodName`),
1. an instance method of some object (`myObject::methodName`),
1. a constructor (`MyClass::new`) or
1. an instance method of an object of some type (`MyObjectType::methodName`)

In all four cases, the return type of the referenced method should match the return type of the required functional interface (unless this is `void`). In the first three cases, the parameters of the two methods would also need to match.

Suppose the parameter required is `(SomeClass object)`, then the implemented method of the functional interface object created would run `MyClass.staticMethodName(object)`, `myObject.methodName(object)` or `new MyClass(object)`.

However, for the last case, the parameters required by the two methods would be different. Suppose the parameter required by the abstract method of the functional interface is `(SomeClass object)` and `SomeClass::doSomething` is given as the required functional interface. This would translate to running `object.doSomething()` in the implemented method.

If the parameters required by the abstract method are `(SomeClass object, SomeArgument arg)`, the implemented method would run `object.doSomething(arg)`. The implemented method runs the referenced method of the first parameter, with the remaining parameters supplied to the referenced method as arguments.

For more information, you should take a look at [this guide written by Esteban Herrera](https://www.codementor.io/eh3rrera/using-java-8-method-reference-du10866vx), who has more than 12 years of experience in Java. The guide provides clear examples to illustrate the different kinds of method references and how it can be used.

#### Lambda Expressions

Moving on to lambda expressions! Lambda expressions are simply a clear and concise way to instantiate an object that implements a functional interface. The expression itself, however, does not contain information about which functional interface it implements, this is deduced by the compiler through the context where it is used.

The structure of a lambda expression is as such : `(parameters) -> <body>`, where the `<body>` can be a block of statement(s),
```java
(int x, int y) -> { return x + y; }
```
or an expression,
```java
(x, y) -> x + y
```
If used in the same context, these two lambda expressions are actually equivalent!

The parameters types of lambda expressions can be inferred by the compiler and are optional. It is actually recommended that parameters types are omitted when writing lambda expressions so as to keep the code concise.

When an expression is used for the body, the result of the expression is returned. The expression can also result in nothing (eg. `(String s) -> System.out.println(s)`), which means that the method expressed by the lambda returns `void`.

When a block is used for the body, the same rules for using or omitting the `return` statement for a normal method applies.

When the lambda has a single parameter, the parentheses surrounding the parameter can also be removed,
```java
x -> x + 10
```

For a lambda expression to be compatible with the required functional interface, the lambda expression must have __the same parameters types__ and __a compatible return type__ as the required functional interface.

Although lambda expressions can be expressed in a block, it does not introduce a new level of scoping. Names in the body of the lambda are interpreted in the same way as its enclosing scope, with the addition of the names of its parameters. `this` and `super` can also be used in the lambda body to refer to the enclosing object and the parent class of the enclosing object.

This also means that lambda expressions are able to access local variables of the enclosing scope as well. However, any local variables accessed by a lambda expression must be final or effectively final (ie. cannot be reassigned another value).

To understand more about lambda expressions, take a look at http://www.lambdafaq.org/ ! The website provides easy-to-understand answers to many questions which you may have on lambda expressions. For a detailed use case of lambda expressions, you can read [this Java tutorial](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html).


#### An Example

Now that we have some understanding on functional interface, method references and lambda expressions, let's consolidate our understanding with an example.

`Comparator<T>` may be a familiar interface to you. In previous Java editions, to create an object extending this interface, you could either instantiate the object from a named class or an anonymous class, both of which need to override the abstract methods in `Comparator<T>`. So the code may look something like this:
```java
// trying to sort students by height
students.sort(new Comparator<Student>() {
    public int compare(Student s1, Student s2) {
      return Double.compare(s1.getHeight(), s2.getHeight());
    }
  });
```
Let's face it. Declaring classes (anonymous or not) is quite a pain. Declaring a named class adds to the number of classes you need to maintain while the syntax of a anonymous class is quite an eyesore.

In Java 8, this interface has become a functional interface (surprise, surprise), so you can write a comparator with a lambda expression.
```java
// trying to sort students by height
students.sort((s1, s2) -> Double.compare(s1.getHeight(), s2.getHeight()));
```
The compiler is able to infer that an object of type `Comparator<Student>` is expected and that the lambda expression fits into the definition for `compare` (the single abstract method), thus creating an instance of type `Comparator<Student>` with the `Double.compare(s1.getHeight(), s2.getHeight())` returned in the implemeneted `compare` method.

By using an lambda expression, the code is much more simplified, and can be now written on a single line. However, the expression in the body is slightly complicated and it may not be easily understood by everyone.

Suppose this comparison is used over and over again in the code. To improve cohesion in the code, you may want to add instance method `compareToByHeight(Student other)` in the `Student` class.
```java
public int compareToByHeight(Student other){
    return Double.compare(height, other.height);
}
```
And you can use a method reference as the functional interface instead.
```java
// trying to sort students by height
students.sort(Student::compareToByHeight);
```
Notice that the code is very easy to understand at a high level and what the intentions of the author can be understood from the code, reducing the need for further documentation. This also makes code more maintainable.

### What is a stream?

Now that we have the basics nailed, let's get started on Streams. Streams are basically sequences of elements. However, when dealing with streams, we are not so interested in where the data of elements is stored, what is currently stored in each element, but rather __what we can do with the elements__.

To use a stream, we need to first construct one. A stream can be obtained from an existing source of elements, such as a collection or an array. We will get into the details of how to do so in the next section.

The methods described in the [Stream API](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html) allows us to perform operations on the elements. The stream operations can be categorised into two kinds: intermediate or terminal.

Intermediate operations are operations which returns a stream. They can be stateless, operating on each element independently, or stateful, where the result of the operation performed on an element depends on other elements in the stream. The intermediate operations can
* reduce the number of elements in the returned stream (eg. `filter`),
* transform the type of the elements in the returned stream (eg. `map`) or
* change the order of the elements in the returned stream (eg. `sorted`)

As a stream is returned from an intermediate operation, you can chain many of these intermediates operations in a single statement. However, intermediate operations are *lazy* and no processing is actually done when an intermediate operation is invoked.

To get any tangible output and to start the processing the operations on the stream, you will need to add a terminal operation. A terminal operation will consume each element in the stream to produce the desired output. Once a stream object is consumed by a terminal operation, it cannot be reused. You would have to construct a new stream object if you want to perform another terminal operation on the stream.

By putting these 3 kind of operations together, we get a stream pipeline, which has some source of elements, performs multiple operations on the elements in the stream, then utilises the elements to get the desired output.

A general guideline is that streams operations should not modify its original data source or be unnecessarily stateful (ie. depending on some variable which may change during the execution of the stream pipeline). Going against this rule can lead to exceptions or unexpected, incorrect behaviour when processing the stream pipeline.

The terminal operations of Streams may also return an [`Optional` object](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html), which is just an container for your desired output. In the case where the stream is empty, the terminal operation will produce an empty optional. This allows developers to differentiate between the case of the terminal operation producing a legitimate `null` result and the case where there is no result due to the absence of elements.

To understand more about Streams, you can read up on the [documentation of the Stream package](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html). They provide clear explanations on how streams work and how you should make use of them.

## Using Java 8 Streams

// stream source

// intermediate operations
  // filter
  // map

// terminal operations
  // collect
  // reduce

### Drawbacks and Pitfalls

In order to effectively utilise streams, one would also need to know the common drawbacks and pitfalls associated with streams. In this section, we will talk about three common pitfalls and how you can avoid them.

#### Long, complicated lambda expressions

Lambda expressions allow us, as developers, to define a function quickly and easily. However, this power can be easily abused, and one might write a long, complicated lambda expression when trying to provide the required functional interface for the stream operation, resulting in code like this:
```java
result = futures.stream()
                .map(HttpService::getFutureValue)
                .map(failureOrResponse -> {
                    return failureOrResponse
                          .right().flatMap(this::parseResponse);
                })
                .map(failureOrResult -> {
                    return failureOrResult.either(
                            failure -> {
                                log.warn(failure.getMessage());
                                return EMPTY_RESULT;
                            },
                            result -> {
                                return result;
                            });
                })
                .reduce(EMPTY_RESULT, Result::union);
```

(adapted from https://www.reddit.com/r/java/comments/2x47wy/java_8_code_style/)

Such code is not only hard to read but also hard to maintain. The general guideline is that __lambda expressions should only be one line long__. If it cannot fit within a single line, the lambda expression is probably not easy to read for people who aren't the author. The code will be much more readable if such lambdas are extracted as methods and this extraction can be easily done with a few clicks in many IDEs such as Eclipse or IntelliJ.

With the lambda expressions extracted as methods `getFailureOrResult` and `getEmptyResultIfFailure`, the earlier code example can be simplified to look like this:
```java
result = futures.stream()
                .map(HttpService::getFutureValue)
                .map(this::getFailureOrResult)
                .map(this::getEmptyResultIfFailure)
                .reduce(EMPTY_RESULT, Result::union);

```
With good method names given to the extracted lambda expressions, the code for the stream operation becomes self-documenting again.


// slow stream performance
    // high-level, harder to optimise
    // developers and compiler are not as good as optimising stream compared to traditional loops
    // parallel streams may spend more time on splitting and merging the work between and from multiple threads, making its performance slower.
    // an issue if performance-critical
//
## Resources

### Functional Interfaces

* https://dzone.com/articles/introduction-functional-1

### Method References

* https://docs.oracle.com/javase/tutorial/java/javaOO/methodreferences.html
* https://www.codementor.io/eh3rrera/using-java-8-method-reference-du10866vx

### Lambda Expressions

* http://www.lambdafaq.org/
* https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html
* http://www.lambdafaq.org/
* http://www.informit.com/articles/article.aspx?p=2303960&seqNum=7

### Stream

* https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html
