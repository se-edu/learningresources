<frontmatter>
  title: An Introduction to Functional Programming
  pageNav: 4
</frontmatter>

<div class="website-content">

# An Introduction to Functional Programming

Authors: [Phang Chun Rong](https://github.com/crphang)

<box id="article-toc">

* [What is Functional Programming‎](#what-is-functional-programming)
* [Functional Programming Languages‎](#functional-programming-languages)
* [Concepts in Functional Programming‎](#concepts-in-functional-programming)
    * [Pure Functions](#pure-functions)
    * [Immutability](#immutability)
* [Techniques in Functional Programming‎](#techniques-in-functional-programming)
    * [Recursion](#recursion)
    * [Higher Order Functions](#higher-order-functions)
* [Advantages of Functional Programming‎](#advantages-of-functional-programming)
* [Disadvantages of Functional Programming‎](#disadvantages-of-functional-programming)
* [Guides on Functional Programming‎](#guides-on-functional-programming)
</box>

## What is Functional Programming

Functional programming is a programming paradigm that treats computation as the evaluation of mathematical function and avoids mutating state and variables. Unlike [imperative programming](https://en.wikipedia.org/wiki/Imperative_programming) languages like C, functional programming languages are [declarative](https://en.wikipedia.org/wiki/Declarative_programming). Even though this demands a shift in mindset from most programmers who are used to imperative form of programming, functional programming paradigm is still [increasingly popular](https://blog.appdynamics.com/engineering/the-most-popular-programming-languages-for-2017/) due to its [advantages](#advantages-of-functional-programming).

## Functional Programming Languages

Functional programming is simply a paradigm and needs to be implemented by programming languages. Below are various [languages](https://en.wikipedia.org/wiki/List_of_programming_languages_by_type#Functional_languages) that have explicit support for the functional programming paradigm:

- Haskell
- Clojure
- Elixir
- Elm
- Scala
- ML (Meta Language) family of languages

While functional programming can be implemented by languages like Java, the languages listed above encourage the functional programming paradigm such as [pure functions](#pure-functions) or even enforce them in the case of Haskell.

## Concepts in Functional Programming

#### Pure Functions

_Pure functions_ is one of the concepts in functional programming. To call a function _pure_, it needs to satisfy two conditions:

1. [Idempotence](https://en.wikipedia.org/wiki/Idempotence) - The function should return the same output for the same input when invoked arbitrarily many times.
1. No side effects - The function should not cause side effects like mutating global state.

To illustrate an example of a pure function. Consider this simple example in Python:

```python
# Pure Function
def add(x, y):
    return x + y
```

Contrast this with an impure function:

```python
# Mutate state
y=0
def add_to_y(x):
    global y
    y = y + x
    return y
```
We see that the impure function violates both conditions. Running `add_to_y` multiple times with the same input X would give different value.

#### Immutability

Another concept in functional programming is immutability. Immutability prevents an object's state to be change after it is created.

Having immutability built into functional programming languages like Haskell helps ensure that functions are _pure_ because mutable variables and states can introduce side-effects.

To know more about immutability in functional languages, you can take a look at:

- [Immutability in Haskell](https://mmhaskell.com/blog/2017/1/9/immutability-is-awesome)
- [Immutability in Elm](https://elmprogramming.com/immutability.html)
- [Why does immutability enable functional programming](https://stackoverflow.com/questions/12207757/why-do-immutable-objects-enable-functional-programming)

## Techniques in Functional Programming

#### Recursion

Pure functional languages do not have loop constructs like imperative languages. This is because `Iteration` usually involves state mutation per iteration. Because functional programming avoids state changes, `Recursion` is a commonly used [technique](https://www.quora.com/Why-dont-pure-functional-programming-languages-provide-a-loop-construct) to replace `Iteration`. An example of replacing state-mutating iterative code to a pure functional recusive code is shown below:

```Python
# Iterative
def getSumOfList(listOfNumbers):
    sum = 0
    for number in listOfNumbers:
        sum += number
    return sum
```

```Python
# Recursive
def getSumOfList(listOfNumbers):
    if len(listOfNumbers) == 0:
        return 0
    return listOfNumbers[0] + getSumOfList(listOfNumbers[1:])
```

Hence, to be able to write functional languages effectively, it means being able to replace Iteration with Recursion. Here are some guides to help you on that:

- [Introduction to Recursion](https://www.topcoder.com/community/data-science/data-science-tutorials/an-introduction-to-recursion-part-1/)
- [Recursion in functional programming](https://dzone.com/articles/functional-programming-recursion)

#### Higher Order Functions

Higher Order Functions are functions that either take functions as parameters, return a function, or both. As functions are first class citizens in functional programming languages, this allows [functions to be passed around like objects and values](https://en.wikipedia.org/wiki/Functional_programming#First-class_and_higher-order_functions).

One example of Higher Order Functions is `map`, which takes in a function and a collection of objects.

```python
length_of_names = map(len, ['Alex', 'Thomas', 'John'])
print(length_of_names) # [4,6,4]
```

`map` takes in 2 parameters, a function and a collection. `map` returns a new collection whose elements are the result of applying the given function on the elements of the given collection. In the given example, `map` returns `[len('Alex'), len('Thomas'), len('John')]`

There are useful functions and techniques that are based off Higher Order Functions in Functional programming.

- [Currying](https://hackernoon.com/javascript-and-functional-programming-currying-pt-4-96e3230782ab)
- [Map, Reduce and Filter](https://medium.freecodecamp.org/higher-order-functions-in-javascript-d9101f9cf528)

## Advantages of Functional Programming

The common question when learning about functional programming is what's the point of having us going through all the trouble of having pure functions, state immutability and absence of loops. Turns out, there are various positive implications of functional programming:

- [More Bug-Free Code](https://www.quora.com/Are-software-written-using-Functional-Programming-less-buggy-more-robust-and-stable). For example famously in Haskell, [if it compiles it usually works](https://wiki.haskell.org/Why_Haskell_just_works).
- [Safer Concurrency](https://softwareengineering.stackexchange.com/questions/293851/what-is-it-about-functional-programming-that-makes-it-inherently-adapted-to-para). Having state immutability and absence of side effects can allow for concurrent code with less concerns of race conditions on mutable objects.
- [And many more](https://alvinalexander.com/scala/fp-book/benefits-of-functional-programming) such as simpler unit testing as functions are guaranteed not to have any side effects.

## Disadvantages of Functional Programming

Functional programming is not a new concept and has been around since the 1950s. Even though it is gaining in popularity today, it is not the predominant programming paradigm used in software applications today. Despite the stated benefits of functional programming, there are some downsides of it that can help explain why it is not the mainstream programming paradigm:

- Lack of imperative data structures. One good example is the [lack of hash map](https://stackoverflow.com/questions/6793259/how-does-one-implement-hash-tables-in-a-functional-language) which is an important performant dictionary.
- Functional programming can be [slower](https://www.quora.com/Do-functional-programming-languages-always-run-slower-than-imperative-language) than optimal Imperative programming for reasons such as data copying due to data immutability.
- And various [other concerns](https://flyingfrogblog.blogspot.sg/2016/05/disadvantages-of-purely-functional.html)

Some of these reasons help in part explain why 'impure' functional languages like Scala is much more popular today than Haskell. Scala is a general purpose language that has interoperability with Java and has support for both imperative and functional programming paradigms. This establishes a [middle ground](https://cacm.acm.org/magazines/2014/4/173220-unifying-functional-and-object-oriented-programming-with-scala/fulltext) for programmers new to functional programming.

## Guides on Functional Programming

Functional programming can be a very different programming paradigm and it definitely takes time to practice to be an expert at it. To learn more about functional programming, take a look at these amazing guides for a deeper understanding:

- A good [overview of functional programming](https://en.wikipedia.org/wiki/Functional_programming)
- A six-part [introduction to functional programming](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536)
- If you are hungry for more, take the [excellent functional programming principles course](https://www.coursera.org/learn/progfun1)
- And to help with the mindset shift for functional programming, take a look at a [practical guide on how to translate an imperative to functional style](https://maryrosecook.com/blog/post/a-practical-introduction-to-functional-programming)
</div>
