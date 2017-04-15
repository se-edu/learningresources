# Java 8 Streams - An Introduction

Authors: Lee Yi Min

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

Besides its brevity, another cool feature of streams is that code can be executed in parallel using your multicore processor. Multiple students can be processed simultaneously, compared to processing only one student at a time with a normal loop. This can help to imrpove the performance of the operation significantly. And the best part is this can be done just by adding a simple method call in the Stream API, so you don't have implement multithreading or worry about how to go about splitting the work for it to work in parallel.

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
* a static method of a class (`MyClass::staticMethodName`),
* an instance method of an object (`myObject::methodName`),
* a constructor (`MyClass::new`) or
* an instance method of an object of some type (`MyObjectType::methodName`)

In all 4 cases, the return type of the referenced method should match the return type of the required functional interface. In the first 3 cases, the parameters of the two methods would also need to match.

However, for the last case, the parameters required by the two methods would be different. Suppose the parameter required by the abstract method of the functional interface is `(SomeClass object)` and `SomeClass::doSomething` is given as the required functional interface. This would translate to running `object.doSomething()` in the implemented method.  
If the parameters required by the abstract method are `(SomeClass object, SomeArgument arg)`, the implemented method would run `object.doSomething(arg)`. The implemented method runs the referenced method of the first parameter, with the remaining parameters supplied to the referenced method as arguments.

For more information, you should take a look at [this guide written by Esteban Herrera](https://www.codementor.io/eh3rrera/using-java-8-method-reference-du10866vx), who has more than 12 years of experience in Java. The guide provides clear examples to illustrate the different kinds of method references and how it can be used.

#### Lambda Expressions

Moving on to lambda expressions, the syntax is ``

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

In Java 8, this interface has become a functional interface (surprise, surprise), which allows you to write the code like this instead!
```java
// trying to sort students by height
students.sort((s1, s2) -> Double.compare(s1.getHeight(), s2.getHeight()));
```
The compiler is able to infer that a object of type `Comparator<Student>` is expected and that the lambda expression fits into the definition for `compare` (the single abstract method), thus creating an instance of type `Comparator<Student>` with the lambda expression implemented as the `compare` method.

To learn more about functional interface, you can visit https://dzone.com/articles/introduction-functional-1

### The Stream API

// source of elements

// intermediate operations

// terminal operations

// optional

## Using Java 8 Streams

// stream source

// intermediate operations
  // filter
  // map
  // flat map

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
