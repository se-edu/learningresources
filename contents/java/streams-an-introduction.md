# Java 8 Streams - An Introduction

Authors: Lee Yi Min

## Overview

In Java 8, we were introduced to new features such as lambda expressions and streams. If you weren't familiar with the concept of functional programming, you might be (like me,) silently screaming in your head as you stare at a chunk of code infused with lambda expressions and stream operations.

However, oftentimes, it's hard not to admire the conciseness of the codes utilising these features. For example, you might want to calculate the mean height of male students given a list of students. Using traditional for-loops, you may write something like this:

```
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

```
double meanMaleHeight = students.toStream()
                                .filter(student -> student.isMale())
                                .mapToDouble(student -> student.getHeight())
                                .average()
                                .orElse(Double.NaN);
```

Not only is the numbers of lines reduced (almost by half!), the code utilising streams is also rather intuitive. First, we filter the students who are male, using a lambda expression (`student -> student.isMale()`). Then, we get the heights of these students and followed by the average value of these heights. If the average value does not exist (which can happen when there are no male students), we store NaN in `meanMaleHeight` instead.

Besides its brevity, another cool feature of streams is that code can be executed in parallel using your multicore processor, which leads to faster performance. And the best part is this can be done just by using a simple method call in the Stream API, you don't have implement multithreading or worry about how to go about splitting the work for it to work in parallel.

It's okay if you have no experience in writing lambda expressions! This guide will step you through the basics of both lambda expressions and streams so that you can start utilising them.

## Getting Started

Before we can get started on Stream, it is imperative to first have some understanding on the concept of functional interface and lambda expressions. Feel free to skip right ahead if you are already familiar with such concepts!

### Functional Interface and Lambda Expressions

If you have tried using the Stream library and looked at its [API page](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html), you would see that many of its methods accept objects of new types such as `Predicate`, `Collector` and more. If you navigate into the APIs of these new types, you would have realised they are merely interfaces (and the APIs themselves aren't giving much help on how you are supposed to make use of these interfaces).

These interfaces are actually functional interfaces, which are simply __interfaces declared with a single abstract method__ (, excluding abstract methods which overrides public methods of `java.lang.Object`). These interfaces provide an easy way for you to somehow reference a method, which could be a existing class method, class constructor or a lambda expression.

Let's see this through an example.

`Comparator<T>` may be a familiar interface to you. In previous Java editions, to create an object extending this interface, you could either instantiate the object from a named class or an anonymous class, both of which need to override the abstract methods in `Comparator<T>`. So the code may look something like this:
```
// trying to sort students by height
students.sort(new Comparator<Student>() {
    public int compare(Student s1, Student s2) {
      return Double.compare(s1.getHeight(), s2.getHeight());
    }
  });
```
Let's face it. Declaring classes (anonymous or not) is quite a pain. Declaring a named class adds to the number of classes you need to maintain while the syntax of a anonymous class is quite an eyesore.

In Java 8, this interface has become a functional interface (surprise, surprise), which allows you to write the code like this instead!
```
// trying to sort students by height
students.sort((s1, s2) -> Double.compare(s1.getHeight(), s2.getHeight()));
```
The compiler is able to infer that a object of type `Comparator<Student>` is expected and that the lambda expression fits into the definition for `compare` (the single abstract method), thus creating an instance of type `Comparator<Student>` with the lambda expression implemented as the `compare` method.

### The Stream API

## Using Java 8 Streams

### Drawbacks and Pitfalls

## Resources
