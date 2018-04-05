# An Introduction to Functional Programming

Authors: [Phang Chun Rong](https://github.com/crphang)

## What is Functional Programming

Functional Programming is a programming paradigm designed based on mathematical functions and avoid mutating state and data. Unlike procedure programming languages like C, Java and Python, Functional Programming languages are [declarative](https://en.wikipedia.org/wiki/Declarative_programming). Even though this demands a shift in mindset from most programmers who are used to imperative form of programming, Functional Programming paradigm is still [increasingly popular](https://blog.appdynamics.com/engineering/the-most-popular-programming-languages-for-2017/) due to its [advantages](#advantages-of-functional-programming).

## Functional Programming Languages

Functional Programming is simply a paradigm and needs to be implemented by programming languages. Below are various [languages](https://en.wikipedia.org/wiki/List_of_programming_languages_by_type#Functional_languages) that have explicit support for Functional Programming paradigm such as:

- Haskell
- Clojure
- Elixir
- Elm
- Scala

While functional programming can be implemented by languages like Java, the languages listed above encourage Functional Programming paradigm such as [pure functions](#pure-functions) or even enforce them in the case of Haskell.

## Concepts in Functional Programming

#### Pure Functions

Pure functions is one of concepts in functional programming. To call a function `pure`, it needs to satisfy two conditions:

1. [Idempotence](https://en.wikipedia.org/wiki/Idempotence) - The function should return the same output for the same input.
1. No Side Effects - The function should not cause side effects like mutating global state.

To illustrate an example of a pure function. Consider this simple example:

```python
# Pure Function
def add(x, y):
    return x + y
```

Contrast this with an impure function:

```python
# Mutate state
y=0
def addXToY(x):
    global y
    y = y + x
    return y
```
We see that the impure function violates both conditions. Running `addXToY` multiple times with the same input X would give different value.

#### Immutability

Another concept in Functional Programming is immutability. When we assign `x=1`, it does not make mathematical sense to have this expression `x:=x+1`.

Having immutability built into Functional Programming languages like Haskell also helps to ensure that functions are `pure` because mutable variables and states can introduce side-effects.

To know more about immutability in functional languages, you can take a look at:

- [Immutability in Haskell](https://mmhaskell.com/blog/2017/1/9/immutability-is-awesome)
- [Immutability in Elm](http://elmprogramming.com/immutability.html)
- [Why does immutability enable Functional Programming](https://stackoverflow.com/questions/12207757/why-do-immutable-objects-enable-functional-programming)

## Techniques in Functional Programming

#### Recursion

Pure functional languages does not have loop constructs that procedural languages does. This is because `Iteration` usually involves state mutation per iteration. Since Functional Programming avoids state changes, `Recursion` is a commonly used [technique](https://www.quora.com/Why-dont-pure-functional-programming-languages-provide-a-loop-construct) to replace `Iteration`.

Hence, to be able to write functional languages effectively, it means being able to replace Iteration with Recursion. Here are some guides to help you on that:

- [Introduction to Recursion](https://www.topcoder.com/community/data-science/data-science-tutorials/an-introduction-to-recursion-part-1/)
- [Recursion in Functional Programming](https://dzone.com/articles/functional-programming-recursion)

#### Higher Order Functions

> Higher Order Functions are functions that take functions as parameters, return functions or both.

Functions are first class citizen in Functional Programming languages, this mean that functions can be passed around like objects and values. 

One example of Higher Order Functions is `map`, which takes in a function and a collection of objects.

```python
length_of_names = map(len, ['Alex', 'Thomas', 'John'])
print(length_of_names) # [4,6,4]
```

`map` takes in 2 parameters, a function and a collection. `map` returns a new collection whose elements are the result of applying the given function on the elements of the given collection. In the given example, `map` returns `[len('Alex'), len('Thomas'), len('John')]`

There are useful functions and techniques that are based off Higher Order Functions in Functional Programming.

- [Currying](https://hackernoon.com/javascript-and-functional-programming-currying-pt-4-96e3230782ab)
- [Map, Reduce and Filter](https://medium.freecodecamp.org/higher-order-functions-in-javascript-d9101f9cf528) 

## Advantages of Functional Programming

The common question when learning about Functional Programming is what's the point of having us going through all the trouble of having pure functions, state immutability and absence of loops. Turns out, there are various positive implications of Functional Programming:

- [More Bug-Free Code](https://www.quora.com/Are-software-written-using-Functional-Programming-less-buggy-more-robust-and-stable)
- [Concurrency](https://softwareengineering.stackexchange.com/questions/293851/what-is-it-about-functional-programming-that-makes-it-inherently-adapted-to-para)
- [And Many More](https://alvinalexander.com/scala/fp-book/benefits-of-functional-programming)

## Disadvantages of Functional Programming

Functional Programming is not a new concept and has been around since the 1950s. Even though it is gaining in popularity today, it is not the predominant programming paradigm used in software applications today. Despite the stated benefits of Functional Programming, there are some downsides of it that can help explain why it is not the mainstream programming paradigm:

- Lack of imperative data structures. One good example is the [lack of hash map](https://stackoverflow.com/questions/6793259/how-does-one-implement-hash-tables-in-a-functional-language) which is an important performant dictionary.
- Functional Programming can be [slower](https://www.quora.com/Do-functional-programming-languages-always-run-slower-than-imperative-language) than optimal Imperative Programming for reasons such as data copying due to data immutability.
- And various [other concerns](http://flyingfrogblog.blogspot.sg/2016/05/disadvantages-of-purely-functional.html)

Some of these reasons help explain why 'impure' Functional languages like Scala is much more popular today than Haskell. Scala is a general purpose language that has interoperability with Java and has support for both popular Object Oriented Programming and Functional Programming paradigm. This establishes a [middle ground](https://cacm.acm.org/magazines/2014/4/173220-unifying-functional-and-object-oriented-programming-with-scala/fulltext) for programmers new to Functional Programming.

## Guides on Functional Programming

Functional Programming can be a very different programming paradigm and it definitely takes time to practice to be an expert at it. To learn more about Functional Programming, I would recommend looking at these amazing guides for a deeper understanding:

- [Introduction to Functional Programming](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536)
- [Functional Programming Principles course](https://www.coursera.org/learn/progfun1)
- [A Practical Guide to Functional Programming](https://maryrosecook.com/blog/post/a-practical-introduction-to-functional-programming)