# Introduction to Haskell

Authors: Thenaesh Elango


# Overview

Haskell is a purely functional programming language with strong, static,
inferred typing.

While Haskell has its roots in academia, its emphasis on purity and
side-effect-free computation makes it a valuable asset in software engineering
contexts. Programs written in Haskell tend to be easy to test, refactor and
debug, with the compiler usually catching all bugs before the program can even
be compiled and run. Consequently, Haskell codebases are extraordinarily stable.

Here's an example of a Haskell program that reads a string of numbers, prints
the sum of the numbers and repeats the process until the string `"quit"` is
entered. This shall serve as our Hello World.

```haskell
-- the entry point of the program
main :: IO ()
main = do
    str <- getLine
    if str == "quit"
        then return ()
        else do
            let sumOfNumbers = sumAllNumbersInString str
            putStrLn $ show sumOfNumbers
            main

sumAllNumbersInString :: String -> Int
sumAllNumbersInString str = sumAll $ extractInts $ tokenize str

-- sums up a list of integers using a higher-order function (the left fold)
sumAll :: [Int] -> Int
sumAll = foldl (+) 0

-- convert each string of digits in a list to an actual integer
extractInts :: [String] -> [Int]
extractInts = fmap read

-- split string by spaces using a built-in function
tokenize :: String -> [String]
tokenize = words
```

Haskell is widely used in a whole range of industries, including banks,
financial companies, technology companies and engineering companies use Haskell
in a variety of systems. A comprehensive list may be found
[here](https://wiki.haskell.org/Haskell_in_industry).


# Getting Started

## Installation

**This tutorial, in general, assumes a _system-wide installation_ of the Haskell
Platform.** This is primarily for simplicity. It is perfectly acceptable to write
small programs or code not intended for production in this manner.

When using Haskell in an actual project, however, it is **strongly-recommended**
to use Stack. Not doing so may cause dependency management to become a nightmare.

### System-Wide Installation

For new users, Haskell may be quickly and easily installed by downloading the
[Haskell Platform](https://www.haskell.org/platform/) for their respective
operating systems. The Haskell Platform contains many common and important
Haskell libraries, in addition to the Glasgow Haskell Compiler (GHC).

At the time of writing, the Haskell Platform has binaries available for all
common operating systems, and many uncommon ones as well.

### Stack Installation

For Haskell projects of significant size, it may be necessary to control the
exact versions of the compiler and libraries used. For such use cases, the
system-wide installation method above may prove unwieldy and inadequate. In
cases like these, it may be preferable to have an entire Haskell environment
just for that project, together with a curated set of libraries.

In such a scenario, [Stack](https://www.haskellstack.org) may come in handy.
Stack is a package manager of sorts for Haskell, similar to NPM. Installation
instructions may be found in the
[Stack Documentation](https://docs.haskellstack.org/en/stable/README/), and is
fairly standard.

A new Stack project may be created and set up with the following:

```sh
# create the project skeleton
stack new ${PROJECT_NAME}

# go into the project source directory
cd ${PROJECT_NAME}

# install GHC for the project
stack setup

# compile the project
stack build

# run the project executable
stack exec ${PROJECT_NAME}-exe
```

The command `stack new` is used to create a new project, which contains a
skeleton already set up. This skeleton includes a `${PROJECT_NAME}.cabal` file,
which contains nearly the entire configuration for the project (compiler/library
versions, modules to be exposed, build targets, etc), and is best thought of as
a sort of `package.json` or `Gemfile` for Stack.

The command `stack setup` downloads and installs GHC. Stack installs GHC
versions into an isolated location in a user's home directory, and does not add
them to the system path. The version used for any particular project depends on
the setting in the project's `${PROJECT_NAME}.cabal` file.

The commands `stack build` and `stack exec` are used to build and run the
project. The executable name for a project named Project is `Project-exe`. This
name is configurable in `Project.cabal`.

The rest of the Stack documentation may be found in the
[official guide](https://docs.haskellstack.org/en/stable/GUIDE/).


## Usage

The command `ghci` may be used to invoke the GHC interpreter. This launches an
[REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) where
Haskell code can be entered and evaluated interactively. This is a very useful
tool when first learning Haskell, and also when debugging code that fails to
compile.

The command `ghc` may be used to compile Haskell code down to machine code. The
invocation of `ghc` is very similar to that of `gcc`.

When using Stack, simply prefix the commands with `stack`.


# Whirlwind Tour

## Types

Haskell is statically typed, meaning that every variable binds to a value of a
specified type. Haskell is also strongly-typed, meaning that every value has a
well-defined type.

### Basic Types

We specify types explicitly by postfixing the variable names with the type.

```haskell
    a :: Int
    a = 5

    -- unbounded integer type, similar to Java BigInt
    b :: Integer
    b = product [1..1000] -- this is the factorial of 1000

    pi :: Double
    pi = 3.141592654
```

Haskell has very powerful type inference engine, so it is possible to omit the
type definitions in most cases.

```haskell
    a = 5
```

### Functions & Currying

Functions, which are just values, have types too. It is considered good practice
in Haskell to specify types for toplevel functions, as a form of documentation,
even though the compiler is likely able to infer types.

```haskell
    square :: Double -> Double
    square x = x * x

    -- computes the hypotenuse of a right triangle given the other two sides
    hypotenuse :: Double -> Double -> Double
    hypotenuse adj opp = sqrt (square adj + square opp)
```
The type definition for `square` is rather obvious. But the type definition of
`hypotenuse` is a little strange. One would expect `(Double, Double) -> Double)`
instead of `Double -> Double -> Double`. The reason is that functions in Haskell
are [curried](https://en.wikipedia.org/wiki/Currying), so a two-parameter
function can be called with a single argument, with a one-parameter function
(that takes in the remaining argument and produces the value) being returned.
`(Double, Double) -> Double` is actually a function that takes in a _single_
2-tuple parameter, which is different from a function that takes in two parameters.

The `->` binds to the right, so `Double -> Double -> Double` may be written as
`Double -> (Double -> Double)` (_a function that takes a double and
returns a function that takes a double and returns a double_).
Calling `hypotenuse 3 4` is also the same as calling `(hypotenuse 3) 4`,
as function application binds to the left.

We may go even further with currying, by fixing some parameters in the function:

```haskell
    -- hypotenuse of a right triangle whose adjacent side is restricted to 3
    hypotenuseWithAdjacent3 :: Double -> Double
    hypotenuseWithAdjacent3 = hypotenuse 3
```

This clearly illustrates how currying can be used to reuse and partially
specialise code as needed. This idiom comes in handy very often in Haskell, as
will be seen later.

It may be of interest to note that **all** functions in Haskell take in at most
one parameter. The illusion of multi-parameter functions is created by currying
and left-binding function calls.

### Algebraic Data Types & Pattern Matching

It is possible to create custom data types, either from nothing or from
existing types.

```haskell
    data TrafficSignal = Red | Amber | Green

    -- define some values of type TrafficSignal, all type-inferred
    redLight = Red
    amberLight = Amber
    greenLight = Green
```

The `TrafficSignal` type is an example of creating data types from nothing.
We call `Red`, `Amber` and `Green` the value constructors and `TrafficSignal`
itself the type constructor. In this case, a `TrafficSignal` has 3 possible
values, `Red`, `Amber` or `Green`.
**Both type and value constructors must start with a capital letter.**

We make use of types in functions by pattern matching on the value constructors.
It is necessary to pattern match on all the value constructors; omitting any
will cause the compiler to complain of non-exhaustive pattern matches.

```haskell
    makeTrafficDecision :: TrafficSignal -> String
    -- leaving any of these out will cause the compiler to complain
    makeTrafficDecision Red = "Stop"
    makeTrafficDecision Amber = "Carefully Proceed"
    makeTrafficDecision Green = "Go"
```

It is also possible to create data types that encapsulate/contain other data types.
The value constructors in this case take parameters instead of being bare.
Pattern matching is done by "expanding" the value constructor.

```haskell
    data HttpRequest = Get String | Post String

    handleRequest :: HttpRequest -> String
    -- the ++ denotes string concatenation in this context
    handleRequest (Get string) = "Get request performed on " ++ string
    -- we use _ to denote that we don't care about the actual value
    handleRequest (Post _) = "Post request not supported"
```

There is also an additional way to declare data types. Suppose we had a C++
class like so:

```cpp
    // we are omitting trivial details like constructors
    class Box {
        double length;
        double breadth;
        double height;
        double density;
    public:
        double getVolume() {
            return this->length * this->breadth * this->height;
        }
        double getMass() {
            return this->density * this->getVolume();
        }
    };
```

We could certainly represent a `Box` as an algebraic data type as follows:

```haskell
    -- NOTE: a value constructor can have the same name as the type constructor
    data Box = Box Double Double Double Double
```

But we are missing key information here. Which `Double` stands for which
attribute? In situations like these, we can use Haskell's record syntax:

```haskell
    -- define box as a record type
    data Box = Box { length :: Double, breadth :: Double, height :: Double, density :: Double }

    getVolume :: Box -> Double
    getVolume (Box { length = l, breadth = b, height = h }) = l * b * h

    getMass :: Box -> Double
    getMass box = getVolume box * density box

    silverBox = Box { length = 5, breadth = 10, height = 15, density = 10.5 }
    goldBox = Box 5 10 15 19.3 -- we can still use normal construction by parameter order
```

There are a few things to note here, other than the syntax itself. When pattern
matching on a record type, we may omit any parameters we do not need (we do not
even need to specify `_`). We may also extract values from the record type by
treating the record parameter names as functions from the record type to the
parameter type. For instance, `density` in the above example is actually a
`Box -> Double` function. Doing `density silverBox` will give the value `10.5`.

### Type Parameters

Consider the division operator on `Double`. We may be tempted to define it with
the type `Double -> Double -> Double`, but the result may be undefined when
dividing by zero. Here's a first stab at a solution to remedy this:

```haskell
    -- represents a value that may or may not exist
    data MaybeDouble = Undefined | Defined Double

    divide :: Double -> Double -> MaybeDouble
    divide _ 0 = Undefined
    divide x y = Defined (x / y)
```

This ensures that division by zero returns a clearly-defined result instead
of something weird.

Now suppose we want to send a HTTP request and retrieve the response data.
This response data may not exist as the server may refuse to return the data.
We can try to solve the problem in the following manner:

```haskell
    data MaybeResponse = NoResponse | GotResponse HttpResponse

    makeRequest :: HttpRequest -> MaybeResponse
    -- implementation details irrelevant
```

We have `MaybeDouble` and `MaybeResponse`, both of which have a common pattern:
they represent possible failure of computation. Naturally, we may wish to abstract
this out. But all the means of abstraction available to us thus far cannot be
used, as we wish to abstract on _types_ rather than values.

For this purpose, Haskell supports _type parameters_, much like how C++ has
templates and Java has generics.

We define the following abstraction of failing computations:

```haskell
    data Maybe t = Nothing | Just t

    divide :: Double -> Double -> Maybe Double
    divide _ 0 = Nothing
    divide x y = Just (x / y)

    makeRequest :: HttpRequest -> Maybe Response
    -- implementation details irrelevant
```

Note that we introduce an additional parameter `t` on the left side of the definition.
This is known as a type parameter, and **must always be lowercase**. This parameter
can then be used in the value constructors as a placeholder for any type that
should be there.

The use of type parameters in this way is similar to the use of generics in Java.
We may think of `Maybe t` as `Optional<T>`, if that helps to understand the role
of `t`.

Note that there can be more than one type parameter. An example is `Either`,
which represents the result of a computation that returns values of different
types on success or failure:

```haskell
    data Either a b = Left a | Right b

    divide :: Double -> Double -> Either String Double
    divide _ 0 = Left "Attempt to divide by zero!"
    divide x y = Right (x / y)
```

Type constructors can be curried just like regular functions or value constructors.
Therefore, `Either String Double` is a concrete type, while `Either String` is
a type constructor that takes in the remaining type.

`Maybe` and `Either` are both defined in the Haskell prelude library.

### Inductive Data Types

We can define a data type in terms of itself. Consider, for instance, a tree.
A tree can be thought of as either an empty tree, or a node with a left subtree
and right subtree attached. We encode it like so:

```haskell
    data Tree t = EmptyTree | Node t (Tree t) (Tree t)
```
Another classic inductive data type is the singly-linked list. The list is
either an empty list or the first element together with the rest of the list.
While not canonical, this is a very common representation of lists in the
functional programming world:

``` haskell
    data List t = EmptyList | Element t (List t)
```

This representation of lists is actually exactly how traditional lists are
defined in Haskell, just with different names and notation as will be seen later.

We are now poised to enter the world of actual functional programming in Haskell.

### Further Reading

* [Type Declarations](https://en.wikibooks.org/wiki/Haskell/Type_declarations)
* [Pattern Matching](https://en.wikibooks.org/wiki/Haskell/Pattern_matching)
* [More on Datatypes](https://en.wikibooks.org/wiki/Haskell/More_on_datatypes)

## Basic Functional Programming

## Typeclasses

## Functors and Applicative Functors

## Monads


# Advanced topics

## Monad Transformers

## Lenses




# Guides

* [Learn You a Haskell](http://learnyouahaskell.com) is a good beginner text
for learning Haskell. It does not have much real-world examples, but does quite
a good job in explaining difficult theoretical concepts (e.g. functors,
applicative functors and monads) well. It is recommended to read  the material
in 3 chunks:
    * 1-6: basic functional programming
    * 7-10: details of the type-system
    * 11-14: monads
* [Real World Haskell](http://book.realworldhaskell.org/) is a rather old text
which is possibly outdated. Still, it shows plenty of examples of how Haskell
may be used in actual real-life scenarios (databases, web programming, etc).
* [Haskell Wikibook](https://en.wikibooks.org/wiki/Haskell) is a comprehensive
Haskell reference. This resource really shines when it comes to aggregating and
covering advanced Haskell topics that typically appear elsewhere in a very
ad-hoc fashion.
* [Typeclassopedia](https://wiki.haskell.org/Typeclassopedia) is a reference
for the major typeclasses contained in the Haskell hierarchical libraries. Use
it to determine which typeclasses are related to which (e.g. every monad is an
applicative functor, which is in turn a functor).
