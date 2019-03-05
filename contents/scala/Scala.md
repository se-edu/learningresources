<frontmatter>
  title: Introduction to Scala
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Introduction to Scala

Author: [Wang Chao](https://github.com/fzdy1914)

**Table of Contents**

* [Overview](#overview)
  * [Characteristic of Scala](#characteristic-of-scala)
    * [Statically Typed](#statically-typed)
    * [Object Oriented](#object-oriented)
    * [Functional](#functional)
* [Getting Started](#getting-started)
* [Why Scala](#why-scala)
  * [Operator Overloading](#operator-overloading)
  * [Mixed-In Trait](#mixed-in-trait)
  * [Type Enrichment](#type-enrichment)
  * [Pattern Matching](#pattern-matching)

# Overview

**Scala** is a modern *multi-paradigm* programming language. It designed to integrate features of 
*object-oriented* and *functional* languages in a *concise*, *elegant*, and *type-safe* way.

## Characteristic of Scala

### Statically Typed
Scala is **statically typed**, which means it enforces type checking ike verifying and enforcing the constraints of 
types at compile-time. 

Scala allows [**type inference**](https://docs.scala-lang.org/tour/type-inference.html), which means it detects the data 
type of an expression automatically. User is not required to annotate redundant type information in Scala. 

In combination, these features provide a clean but reliable programming basis for the user.

### Object Oriented
Scala is a **pure OOP language**, which means every value in it is an object, including functions and primitives.

Types and behavior of objects are described by *classes* and *traits*. 

Classes are extended by *subclassing* and a flexible *mixin-based* mechanism to avoid the problems of multiple inheritance.

Classes cannot have *static members*. A singleton object with same name of the class can be used to achieve the same effect. 
A singleton object is basically a class that can have only one instance.

### Functional
Scala is a **Functional Programming language** in the sense that every function in Scala is a value. 

Scala provides a lightweight syntax for defining [*anonymous functions*](https://docs.scala-lang.org/tour/basics.html#functions).

It also support many other features of FP including [*higher-order functions*](https://docs.scala-lang.org/tour/higher-order-functions.html), 
[*nested functions*](https://docs.scala-lang.org/tour/nested-functions.html), [*currying*](https://docs.scala-lang.org/tour/multiple-parameter-lists.html), 
[*pattern matching*](https://docs.scala-lang.org/tour/pattern-matching.html), *immutability* and *lazy evaluation*.

# Getting Started
There are mainly two ways to work in Scala.

1. Using the **INTELLIJ IDE**. Refer to the [instructions](https://docs.scala-lang.org/getting-started-intellij-track/getting-started-with-scala-in-intellij.html) for more details.
2. Using the **command line**. Refer to the [instructions](https://docs.scala-lang.org/getting-started-sbt-track/getting-started-with-scala-and-sbt-on-the-command-line.html) for more details.

# Why Scala
There are mainly two benefits of Scala, **simple syntax** and **multi-paradigm**. 

*Simple syntax* allows us to write more simple and elegant code. 

*Multi-paradigm* allows us to use an OOP language while using the feature in functional programming. 

Following are some examples to illustrate the benefit of Scala:

## Operator Overloading
All operators in Scala such as +, -, *, /. are actually methods.

Normally, we write addition in following way:

`val a = 1 + 2`

However, in Scala, it will be interpreted as following:

`val a = 1.+(2)`

Knowing that, we are able to overload operators just like method overloading. It allows us to redefine the behavior of
arithmetic operators and give them meaning for our own custom classes.

Following is an elegant *ComplexInt* class which uses **operator overloading**.

```
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

```
val a = ComplexInt(1, 1)
val b = ComplexInt(1, -1)
val c = a * b
val d = -c + a
```

In other languages like Java, the similar operations of the same class may need to performed like following:

```
ComplexInt a = new ComplexInt(1, 1);
ComplexInt b = new ComplexInt(1, -1);
ComplexInt c = a.times(b);
ComplexInt d = (c.negative()).add(a);
```


## Mixed-In Trait

## Type Enrichment

## Pattern Matching



</div>
