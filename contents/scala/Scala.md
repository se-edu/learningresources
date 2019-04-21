<frontmatter>
  title: Introduction to Scala
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Introduction to Scala

**Author(s): [Wang Chao](https://github.com/fzdy1914)**

Reviewers: [Jiang Chunhui](https://github.com/Adoby7), [Wang Junming](https://github.com/junming403)

## Overview

> Scala combines object-oriented and functional programming in one concise, high-level language. Scala's static types help avoid bugs in complex applications, and its JVM and JavaScript runtimes let you build high-performance systems with easy access to huge ecosystems of libraries. -- [Scala Website](https://www.scala-lang.org/)

Let's look at some characteristics of Scala mentioned in the above quote:

* **Scala supports _object-oriented_ programming**: Every value in Scala is an object, including functions and primitives. Classes are extended by *subclassing* and a flexible *mixin-based* mechanism to avoid the problems of multiple inheritance.
* **Scala supports _functional_ programming**: Every function in Scala is a value. Scala provides a lightweight syntax for defining [*anonymous functions*](https://docs.scala-lang.org/tour/basics.html#functions). It also support many other features of FP including [*higher-order functions*](https://docs.scala-lang.org/tour/higher-order-functions.html), [*nested functions*](https://docs.scala-lang.org/tour/nested-functions.html), [*currying*](https://docs.scala-lang.org/tour/multiple-parameter-lists.html), [*pattern matching*](https://docs.scala-lang.org/tour/pattern-matching.html), *immutability* and *lazy evaluation*.
* **Scala is _statically typed_**: It enforces type checking like verifying and enforcing the constraints of types at compile-time. 
* **Scala can work the Java and JavaScript runtimes**: Scala provides language interoperability with Java, so that libraries written in Java can be referenced directly in Scala. Also, by *[Scala.js](http://www.scala-js.org/)*, Scala code can be easily compiled to JavaScript.

As Scala supports both OOP and FP paradigms, it is considered a *multi-paradigm* language. Furthermore, Scala proponents claim it to have a simple syntax that can produce elegant code.

## Noteworthy Scala Features

Following are some noteworthy Scala features:

### Type Inference
Scala allows [**type inference**](https://docs.scala-lang.org/tour/type-inference.html), which means it detects the data type of an expression automatically. The user is not required to annotate redundant type information in Scala.

Type Inference Example:
```scala
var x = "foo"
var y = 1.5
val z = List(1, 2, 3)

x = 3 // Error: type mismatch
x = "bar" // OK
```

### Operator Overloading
All operators in Scala such as `+`, `-`, `*`, `/` are actually methods.

Normally, we write addition in following way:

`val a = 1 + 2`{.scala}

However, in Scala, it will be interpreted as following:

`val a = 1.+(2)`{.scala}

Knowing that, we are able to overload operators just like method overloading. It allows us to redefine the behavior of arithmetic operators and give them meaning for our own custom classes.

Following is a *ComplexInt* class which uses operator overloading.

```scala
case class ComplexInt(real: Int, im: Int) {
  def + (other: ComplexInt) = ComplexInt(real + other.real, im + other.im)
  def * (other: ComplexInt) = ComplexInt(
    real * other.real - im * other.im,
    real * other.im + im * other.real
  )
  def unary_- = ComplexInt(-real, -im)
  def - (other: ComplexInt) = this + -other

  override def toString = real + " + " + im + "i"
}
```

We are able to use the class like below:

```scala
val a = ComplexInt(1, 1)
val b = ComplexInt(1, -1)
val c = a * b
val d = -c + a
```

In contrast, this is how the same operations would look like if written in Java:

```java
ComplexInt a = new ComplexInt(1, 1);
ComplexInt b = new ComplexInt(1, -1);
ComplexInt c = a.times(b);
ComplexInt d = (c.negative()).add(a);
```


### Mixed-In Trait
In scala, types and behavior of objects are described by *classes* and *traits*. 

[*Traits*](https://docs.scala-lang.org/tour/traits.html) are used to share interfaces and fields between classes, which are similar to Java's interfaces. 

Classes are extended by *subclassing* and a flexible *mixin-based* mechanism to avoid the problems of multiple inheritance.

Classes cannot have *static members*. A singleton object with same name of the class can be used to achieve the same effect. A singleton object is basically a class that can have only one instance.

Following is the piece of code to show the uniqueness of OOP in Scala:
```
abstract class ParentTrait {
  // abstract
  def print()
}

class Sample extends ParentTrait {
  override def print() {
    println("print in Sample")
  }
}

trait TraitA extends ParentTrait {
  abstract override def print() {
    println("print in TraitA")
    super.print()
  }
}

trait TraitB extends ParentTrait {
  abstract override def print() {
    println("print in TraitB")
    super.print()
  }
}

object Sample {
  def main(args: Array[String]): Unit = {
    val example = new Sample with TraitA with TraitB
   example.print()
  }
}
```
The following is the execution result of above code:
```
print in TraitB
print in TraitA
print in Sample
```
The explanation is that the call to `print()` firstly executes the code in `TraitB`, which is the last trait mixed in. Then through the `super()` calls, it threads back through the other mixed-in traits. And eventually to the code in `Sample`. Even though none of the traits inherited from one another, it works properly. 

This is similar to the [decorator pattern](https://en.wikipedia.org/wiki/Decorator_pattern) but is more *concise* and less *error-prone*. In other languages, a similar effect could be achieved with a long linear chain of inheritance. But the disadvantage is that for every possible combination of the mix-ins, we need to declare an inheritance chain for it.


### Type Enrichment
Have you ever imagine that you are able to add new function to existing library? An implicit class in Scala can help you to do so. This technique allows new methods to be added to an existing class using an add-on library such that only code that imports the add-on library gets the new functionality, and all other code is unaffected. See the code below:
```
object MyExtensions {
  implicit class IntParity(i: Int) {
    def isEven = i % 2 == 0
    def isOdd  = !isEven
  }
}

import MyExtensions._  // bring implicit enrichment into scope
4.isEven  // -> true
4.isOdd   // -> false
```
This is the implicit class that extend the library of class Int. The name of the class does not matter. After importing this class, we can use `isEven()` and `isOdd()` method just as if it has been declared in its own class. 


### Pattern Matching
Scala has built-in support for **pattern matching**, which can be thought of as a more extensible version of a **switch** statement, where arbitrary data types can be matched. The `match` operator is used to do **pattern matching** .

There are many kinds of pattern matching in Scala, let's start with a simplest pattern matching function:
```
def matchTest(x: Int): String = x match {
  case 0 => "zero"
  case 1 => "one"
  case 2 => "two"
  case _ => "many"
}

matchTest(0)  // zero
matchTest(3)  // many
matchTest(1)  // one
```
The parameter x is the left operand of the `match` operator and on the right is an expression with four cases. Each case expression is tried to see if it will match, and the first match will be returned. The last case _ is a "catch all" case which can match all inputs.

The **pattern matching** in Scala also includes Case Class Matching, List Matching, Type Matching, Tuple Matching and etc. You can see [PATTERN MATCHING](https://docs.scala-lang.org/tour/pattern-matching.html) for more details.

Following is an example of QuickSort algorithm to show how strong is Scala's pattern matching:

```
def qsort(list: List[Int]): List[Int] = list match {
  case Nil => Nil
  case pivot :: tail =>
    val (smaller, rest) = tail.partition(_ < pivot)
    qsort(smaller) ::: pivot :: qsort(rest)
}
```

The idea here is that we *partition* a list recursively, sort each part and combine the results together.(See [QuickSort](https://en.wikipedia.org/wiki/Quicksort) for the details of the algorithm)

In this case, `Nil` only matches the object `Nil` (Empty list). But `pivot :: tail` matches a non-empty list, and it will destructure the list according to the pattern given. 
In this case, the code will have a variable `pivot` holding the head of the list, and another variable `tail` holding the tail of the list.
 
In local variable declarations, tuple matching is used. The return value of the call to `tail.partition` is a tuple. 
In this case, the code will have a variable `smaller` holding the first element of the tuple, and another variable `rest` holding the second element of the list. 

## Getting Started
There are mainly two ways to work in Scala.

1. Using the **IDE**. Refer to the [instructions](https://docs.scala-lang.org/getting-started-intellij-track/getting-started-with-scala-in-intellij.html) for more details.
2. Using the **command line**. Refer to the [instructions](https://docs.scala-lang.org/getting-started-sbt-track/getting-started-with-scala-and-sbt-on-the-command-line.html) for more details.

A more detailed tutorial can be found here: [Tour of Scala](https://docs.scala-lang.org/tour/tour-of-scala.html)

### Learning Path
Following is a suggested learning path to follow to learn Scala:

* [Formatting](http://twitter.github.io/effectivescala/index.html#Formatting): Whitespace, Naming, Imports, Braces, Pattern matching, Comments
* [Types and Generics](http://twitter.github.io/effectivescala/index.html#Types%20and%20Generics): Return type annotations, Variance, Type aliases, Implicits
* [Collections](http://twitter.github.io/effectivescala/index.html#Collections): Hierarchy, Use, Style, Performance, Java Collections
* [Concurrency](http://twitter.github.io/effectivescala/index.html#Concurrency): Futures, Collections
* [Control structures](http://twitter.github.io/effectivescala/index.html#Control%20structures): Recursion, Returns, for loops and comprehensions, require and assert
* [Functional programming](http://twitter.github.io/effectivescala/index.html#Functional%20programming): Case classes as algebraic data types, Options, Pattern matching, Partial functions, Destructuring bindings, Laziness, Call by name, flatMap
* [Object oriented programming](http://twitter.github.io/effectivescala/index.html#Object%20oriented%20programming): Dependency injection, Traits, Visibility, Structural typing
* [Error handling](http://twitter.github.io/effectivescala/index.html#Error%20handling): Handling exceptions
* [Garbage collection](http://twitter.github.io/effectivescala/index.html#Garbage%20collection)

### Where to go from here
Some useful resources are listed below if you want to learn more about Scala:

* [Scala Exercises](https://www.scala-exercises.org/) is a series of lessons and exercises created by 47 Degrees. 
It's a great way to get a brief introduction to Scala while testing your knowledge along the way.

* [Functional Programming Principles in Scala](https://www.coursera.org/course/progfun), free on Coursera. 
This is a course about functional programming given by Martin Odersky himself. 
You can access the course material and exercises by signing up for the course.

* [allaboutscala](http://allaboutscala.com/) provides detailed tutorials for beginners.

More details can be find here: [Scala Learning Resources](https://scala-lang.org/documentation/learn.html).
</div>
