<frontmatter>
  title: Introduction to Haskell
  pageNav: 3
</frontmatter>

<div class="website-content">

# Introduction to Haskell

Author: Thenaesh Elango

<box id="article-toc">

* [Overview](#overview)
* [Getting Started](#getting-started)
    * [Installation](#installation)
        * [System-Wide Installation](#system-wide-installation)
        * [Stack Installation](#stack-installation)
    * [Usage](#usage)
* [Whirlwind Tour](#whirlwind-tour)
    * [Types](#types)
        * [Basic Types‎](#basic-types)
        * [Functions & Currying‎](#functions-and-amp-currying)
        * [Algebraic Data Types & Pattern Matching‎](#algebraic-data-types-and-amp-pattern-matching)
        * [Type Parameters‎](#type-parameters)
        * [Inductive Data Types‎](#inductive-data-types)
        * [Further Reading‎](#further-reading)
    * [General Functional Programming](#general-functional-programming)
        * [Functions‎](#functions)
        * [Recursion‎](#recursion)
        * [Lists‎](#lists)
        * [List Processing - Fold‎](#list-processing-fold)
        * [List Processing - Map & Filter‎](#list-processing-map-and-amp-filter)
        * [Programming with Other Inductive Data Types‎](#programming-with-other-inductive-data-types)
        * [Further Reading‎](#further-reading-2)
    * [Typeclasses](#typeclasses)
        * [Defining and Instantiating Typeclasses‎](#defining-and-instantiating-typeclasses)
        * [Adding Typeclass Constraints to Functions‎](#adding-typeclass-constraints-to-functions)
        * [Instantiating Typeclasses with Parameterized Type Constructors‎](#instantiating-typeclasses-with-parameterized-type-constructors)
        * [Further Reading‎](#further-reading-3)
* [Common Haskell Idioms](#common-haskell-idioms)
    * [Functors](#functors)
    * [Applicative Functors](#applicative-functors)
    * [Monads](#monads)
* [Guides](#guides)
</box>


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
    -- input:      x of type Double
    -- output: x * x of type Double
    square :: Double -> Double
    square x = x * x

    -- computes the hypotenuse of a right triangle given the other two sides
    hypotenuse :: Double -> Double -> Double
    hypotenuse adj opp = sqrt (square adj + square opp)
```

If the above syntax is confusing and the comments insufficient, the reader may
wish to consult the detailed introduction to Haskell syntax
[here](http://learnyouahaskell.com/starting-out).

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

## General Functional Programming

### Functions

A function may be defined in one of several ways. We illustrate the various
syntaxes for defining a function below, with more details
[here](http://learnyouahaskell.com/syntax-in-functions) if needed:

```haskell
    sumOfSquares :: Double -> Double -> Double
    -- standard definition
    sumOfSquares x y  = (x * x) + (y * y)
     -- lambda function
    sumOfSquares = \x y -> (x * x) + (y * y)

    fizzBuzz :: Int -> Either String Int
    -- the horrible, disgusting, but still perfectly correct way
    fizzBuzz x = case x `mod` 15 == 0 of
                    True -> Left "fizzbuzz"
                    False -> case x `mod` 3 == 0 of
                        True -> Left "fizz"
                        False -> case x `mod` 5 == 0 of
                            True -> Left "buzz"
                            False -> Right x
    -- far more elegant way using guard patterns
    fizzBuzz x
        | x `mod` 15 = Left "fizzbuzz"
        | x `mod` 3 = Left "fizz"
        | x `mod` 5 = Left "buzz"
        | otherwise = Right x
```

### Recursion

Recursion is one of the fundamental themes of functional programming. It is the
ability of a function to call itself.

```haskell
    -- Time: O(n)
    -- Space: O(n)
    factorial :: Integer -> Integer
    factorial 0 = 1
    factorial n = n * factorial (n - 1)

    -- Time: O(2^n)
    -- Space: O(n), may vary due to lazy evaluation
    fibonacci :: Integer -> Integer
    fibonacci 0 = 0
    fibonacci 1 = 1
    fibonacci n = fibonacci (n - 2) + fibonacci (n - 1)
```

While Haskell has no primitive loop structures, looping can be simulated by
recursion. While attempting this in languages in C may cause a stack overflow,
Haskell avoids this via [tail-call optimisation](https://en.wikipedia.org/wiki/Tail_call),
which can be applied to recursive calls that meet certain requirements.

```Haskell
    -- Time: O(n)
    -- Space: O(1)
    factorial :: Integer -> Integer
    factorial = factorial' 1 where
        factorial' p 0 = p
        factorial' p n = factorial' (p * n) (n - 1)

    -- Time: O(n)
    -- Space: O(1)
    fibonacci :: Integer -> Integer
    fibonacci 0 = 0
    fibonacci 1 = 1
    fibonacci n = fibonacci' 0 1 n where
        fibonacci a _ 0 = a
        fibonacci' a b n = fibonacci' b (a + b) (n - 1)
```

We can safely omit the types in the inner function definitions due to type
inference. Also note how we freely use currying in the `factorial` definition.

### Lists

As described earlier, a list is an inductive data type, defined as either the
empty list or an element concatenated with the rest of the list. The actual list
data type is
```haskell
    data [] t = [] | (:) t ([] t)
```
where `:` is an infix value constructor.

> **IMPORTANT: Infix Functions**
>
> Any function (a value constructor is really just a function) that takes in two parameters
> whose name consists of nothing but symbols is infix by default.
>
> An infix function like `+` may be used in prefix form by enclosing in parentheses.
> For instance, `1 + 1` is the same as `(+) 1 1`.<br>
> In the type definition, the prefix form must be used i.e. `(+) :: Int -> Int -> Int`.<br>
> In the function definition, either is acceptable.
>
> [Find out more.](https://wiki.haskell.org/Infix_operator)
>
> We will use this concept freely from now on.

Here are several ways to define a list `xs :: [Int]` containing 2,4,6,8 in that order:

```haskell
    -- the crazy way, using prefix notation directly from the list definition
    xs = (:) 2 ((:) 4 ((:) 6 ((:) 8 [])))

    -- using infix syntax for (:), still annoying to write
    xs = 2:(4:(6:(8:[])))

    -- taking advantage of binding rules for (:), noiseless and easier to understand at a glance
    xs = 2:4:6:8:[]

    -- using varying amounts of list syntactic sugar provided by the compiler
    xs = 2:4:6:[8]
    xs = 2:4:[6,8]
    xs = 2:[4,6,8]
    xs = [2,4,6,8]
```

The last representation is most commonly used, while the second last is often
used when pattern matching on lists. The rest are almost never seen in practice.
However, it is hoped that this pedantic exercise helps the reader understand
the true nature of lists: an ordinary inductive data type with some compiler
syntactic sugar tacked on.

### List Processing - Fold

List processing is a very important part of elementary functional programming.
This is due to the fact that lists can store large amounts of data, and it is
very easy to define powerful abstractions to slice and dice that data in ways
typically unknown in imperative programming.

One common idiom is to loop over a list and aggregate their values.

It is possible to run over a list and sum their values recursively like so:

```haskell
    sumList :: [Int] -> Int
    sumList [] = 0
    sumList (x:xs) = x + sumList xs
```

Note the infix pattern match `(x:xs)` as opposed to `((:) x xs)`. What if we wish
to take the product of the elements instead of a sum? Then we would write:

```haskell
    prodList :: [Int] -> Int
    prodList [] = 1
    prodList (x:xs) = x * prodList xs
```

It is clear that some abstraction is in order here. The functions are almost
identical except for the aggregating function used and the initial value (0 for
sum, 1 for product). We can write a generalised aggregating function:

```haskell
    -- 1st parameter is the aggregating function (e.g. (+) or (*))
    -- 2nd parameter is the initial value
    -- 3rd parameter is the list to aggregate
    aggregate :: (Int -> Int -> Int) -> Int -> [Int] -> Int
    aggregate _ initial [] = initial
    aggregate op initial (x:xs) = op x (aggregate op initial xs)
```

This is better, but perhaps we could generalise this even further beyond `Int`.
We then arrive at the following, by simply changing the type signature:

```haskell
    -- 1st parameter is the aggregating function (e.g. (+) or (*))
    -- 2nd parameter is the initial value
    -- 3rd parameter is the list to aggregate
    aggregate :: (a -> b -> b) -> b -> [a] -> b
    aggregate _ initial [] = initial
    aggregate op initial (x:xs) = op x (aggregate op initial xs)
```

This function is known as `foldl` in the Haskell prelude library, and there is
also a variant called `foldr` that does the aggregation from the right instead.

### List Processing - Map & Filter

One may wish to take in a list, transform every element in the list, and output
the resulting list. This is known as a map, and may be defined as:

```haskell
    map :: (a -> b) -> [a] -> [b]
    map _ [] = []
    map f (x:xs) = (f x):(map f xs)
```

The type definition itself contains a wealth of information. The `map` function
takes in a "transformer", the list to be transformed, and return the transformed
list. An example of its usage would be:

```haskell
    -- xs is [1,4,9,16]
    xs = map (\x -> x * x) [1,2,3,4]
```

One may also wish to remove certain elements, that fail some predicate, from a
given list. This is known as a filter:

```haskell
    filter :: (t -> Bool) -> [t] -> [t]
    filter _ [] = []
    filter predicate (x:xs)
        | predicate x == True = x:xs
        | otherwise = xs
```

This example uses guard patterns. An example of using filter would be:

```haskell
    -- xs is [2,4]
    xs = filter (\x -> x `mod` 2 == 0) [1,2,3,4]
```

It is left as an exercise for the reader to implement `map` and `filter`
in terms of `foldl` (or `aggregate` as defined above, which is the same).

### Programming with Other Inductive Data Types

Recursion is a natural fit with inductive data types other than lists. One
example would be finding an element in a binary tree:

```haskell
    find :: Tree Int -> Int -> Bool
    find EmptyTree _ = False
    find (Node x left right) target
        | x == target = True
        | x < target = find left target
        | x > target = find right target
```

The above runs in O(log n) as long as the tree is balanced.

### Further Reading

* [Function Syntax](http://learnyouahaskell.com/syntax-in-functions)
* [Higher-Order Functions](http://learnyouahaskell.com/higher-order-functions)


## Typeclasses

Typeclasses are essentially contracts/constraints imposed on types. They are
similar to how Java interfaces are constraints imposed on Java classes. When
used properly, they are an extremely powerful tool in helping to structure code.

### Defining and Instantiating Typeclasses

```haskell
    -- "class" here has nothing to do with OOP
    class Eq t where
        (==) :: t -> t -> Bool
        (!==) :: t -> t -> Bool
        -- this ensures that we don't have to define (!==) separately
        a != b = not (a == b)
```

We have just defined a typeclass called `Eq`. As its name probably suggests,
this typeclass is used when we wish to define the meaning of equality on types.
We then _instantiate_ the typeclass with the `TrafficSignal` type, like so:

```haskell
    instance Eq TrafficSignal where
        -- note that these are infix function DEFINITIONS
        -- we can define infix operators directly in infix notation
        Red == Red
        Amber == Amber
        Green == Green
```

We have thus defined `(==)` completely for `TrafficSignal`. Note that `(!=)`
now comes for free, since we have defined it in terms of `(==)` in the typeclass
itself.

Here's another example:

```haskell
    instance Eq (List t) where
        EmptyList == EmptyList
        (Element x xs) == (Element y ys) = (x == y) && (xs == ys)
```

Here, we define the equality of a list in terms of its underlying elements.
This seems reasonable. However, running this program will give an error. This
is because we are attempting to compare the underlying elements (of type `t`)
using `(==)`, which is not guaranteed to be defined on `t`.

The solution, in this case, is to enforce a typeclass constraint prerequisite
on `t` by writing:

```haskell
    instance (Eq t) => Eq (List t) where
        -- as before
```

Here is an example of `Eq` being defined on `Tree`s:

```haskell
    instance (Eq t) => Eq (Tree t) where
        EmptyTree == EmptyTree
        (Node x left right) == (Node x' left' right') = (x == x') && (left == left') && (right == right')

    tree1 = Node 1 (Node 2 EmptyTree) (Node 3 EmptyTree)
    tree2 = Node 1 (Node 2 EmptyTree) EmptyTree
    tree3 = Node 1 (Node 2 EmptyTree) EmptyTree

    -- some experiments
    tree1 == tree2 -- False
    tree2 == tree3 -- True
    tree3 != tree1 -- True
```

We present another common typeclass called `Ord`, which defines order for a type:

```haskell
    -- anything that instantiates Ord must also instantiate Eq
    -- this makes the typeclass definitions simpler as (==) is already provided and can be used
    class (Eq t) => Ord t where
        -- the only one we actually need to implement when instantiating
        (<) :: t -> t -> Bool

        -- we predefine these and can then get them all for free
        (>) :: t -> t -> Bool
        a > b = not ((a < b) || (a == b))

        (<=) :: t -> t -> Bool
        a <= b == (a < b) || (a == b)

        (>=) :: t -> t -> Bool
        a >= b = not (a < b)
```

### Adding Typeclass Constraints to Functions

Consider the following function to check if the elements in the following list
are all in ascending order:

```haskell
    isAscending :: [t] -> Bool
    isAscending [] = True -- handle 0-element lists
    isAscending (x:[]) = True -- handle 1-element lists
    isAscending (x:y:xs) = (x < y) && isAscending (y:xs) -- recursive case
```

This function seems reasonable, except for one minor detail: we (and the compiler)
are not sure if `t` can be compared using `(<)`!. To remedy this, we need to
explicitly state that `t` instantiates `Ord`, thereby allowing the use of `(<)`.
We do this by adding the constraint in the function type definition:

```haskell
    isAscending :: (Ord t) => [t] -> Bool
```

We can now try out the `isAscending` function:

```haskell
    isAscending [1,2,4,3] -- False
    isAscending [1,2,3,4,5] -- True
    isAscending [] -- True
```

### Instantiating Typeclasses With Parameterized Type Constructors

Up to this point, we have been instantiating typeclasses with concrete types,
such as `TrafficSignal` and `Tree t`. It is also possible to instantiate
typeclasses with **parameterized** type constructors like `Tree` and `List`.

Consider the following typeclass `Container` that is instantiated by types that
have some notion of constituent elements and size. For instance, a `List` has a
length and contains elements of some type. A `Tree` has nodes and a size (number
of nodes). The length is independent of type of element contained within.

```haskell
    class Container s where
        -- t is an arbitrary unconstrained type
        size :: s t -> Int
```

We can instantiate `Container` with `Tree` and `List`. These are parameterized
type constructors, not concrete types. We can even instantiate with `Maybe`.

```haskell
    instance Container Tree where
        size EmptyTree = 0
        size (Node _ left right) = 1 + size left + size right

    instance Container List where
        size EmptyList = 0
        size (Element _ restOfList) = 1 + size restOfList

    instance Container Maybe where
        size Nothing = 0
        size (Just _) = 1
```

As an exercise, the reader may wish to redefine the size of a `Tree` to mean
"height of tree" rather than "number of nodes". It is necessary to instantiate
`Container` with `Tree` differently to achieve this. The function
`max :: (Ord a) => a -> a -> a` may come in handy (`Int` is an instance of `Ord`).

### Further Reading

* [More on Creating Typeclasses](http://learnyouahaskell.com/making-our-own-types-and-typeclasses)
* [Collection and Relationship between Standard Typeclasses](https://wiki.haskell.org/Typeclassopedia)


# Common Haskell Idioms

## Functors

Consider the `map` function previously defined. The type
of `map` is `(a -> b) -> [a] -> [b]`, which means that it operates only on lists.
We may imagine extending maps to `Tree`s and `Maybe`s in the following manner:

```haskell
    map :: (a -> b) -> Tree a -> Tree b
    map :: (a -> b) -> Maybe a -> Maybe b
```

The two type definitions above look very similar and suggest a generalization:
types that can be mapped over. We call such types _functors_, and can represent
their behaviour with a typeclass.

```haskell
    class Functor f where
        -- f is a type constructor that takes in one type parameter
        fmap :: (a -> b) -> f a -> f b


    instance Functor Tree where
        fmap _ EmptyTree = EmptyTree
        fmap f (Node x left right) = Node (f x) (fmap f left) (fmap f right)

    instance Functor Maybe where
        fmap _ Nothing = Nothing
        fmap f (Just x) = Just (f x)
```

We can then map over values of any functor:

```haskell
    sq x = x * x

    fmap sq (Just 5) -- returns Just 25
    fmap sq Nothing -- returns Nothing

    fmap sq (Node 1 (Node 2 EmptyTree) (Node 3 (Node 4 EmptyTree) EmptyTree))
    -- returns (Node 1 (Node 4 EmptyTree) (Node 9 (Node 16 EmptyTree) EmptyTree))
```

More information about functors, including the functor laws,
may be found [here](https://en.wikibooks.org/wiki/Haskell/The_Functor_class).

## Applicative Functors

An _applicative functor_ is a functor that allows for a more advanced type of
mapping. We shall jump straight into the (abridged) typeclass definition and the
example of `Maybe` as an applicative functor:

```haskell
    class (Functor f) => Applicative f where
        pure :: a -> f a
        (<*>) :: f (a -> b) -> f a -> f b -- generalized map function

    instance Applicative Maybe where
        pure x = Just x
        Nothing <*> _ = Nothing
        _ <*> Nothing = Nothing
        Just f <*> Just x = Just (f x)
```

Applicative functors have the concept of _lifting_, embodied in `pure`, where
a value is taken and placed in the context of a functor. For instance, in the
context of `Maybe`, `pure 5` returns the value
`Just 5`.

Applicative functors allow a more general form of mapping, where it is possible
to use an N-parameter function to map over N functors. To understand the value
of this, consider the following code:

```haskell
    euclideanDistance :: Double -> Double -> Double -> Double
    euclideanDistance x y z = sqrt ((x * x) + (y * y) + (z * z))

    (pure euclideanDistance) <*> Just 1 <*> Just 2 <*> Just 3
    -- returns Just 3.7416573867739413
```

The above code can be written with just `fmap` in the ordinary `Functor` class,
but will involve incredible contortions.

More information about applicative functors can be found
[here](https://en.wikibooks.org/wiki/Haskell/Applicative_functors). There is a
lot of additional functionality available in the `Applicative` typeclass. We
have barely scratched the surface.

## Monads

No Haskell tutorial will be complete without an introduction to the fabled
monad. Monads have been described with various analogies, as well as with
notorious phrases from category theory like "a monad is a monoid in the category
of endofunctors".

None of these are useful for the software engineer, so we dispense with them and
opt for just showing the code:

```haskell
    class (Applicative m) => Monad m where
        -- this function, called "bind", is at the heart of the monad
        (>>=) :: m a -> (a -> m b) -> m b

        -- we could actually just use pure, but return is here for historical reasons
        return :: a -> m a
        return = pure
```

A monad is essentially an applicative functor that allows for operations to be
chained together with a value carried in the background. To consider this, let
us consider the familiar case of `Maybe`, which is a monad.

```haskell
    instance Monad Maybe where
        Nothing >>= _ = Nothing
        (Just x) >>= f = Just (f x)

    -- maybeSomeValue is Just 50
    maybeSomeValue = Just 5 >>= (\x -> Just (x * x) >>= (\x -> Just (x + x)))
```

Essentially, the bind function allows for values carried inside the monad (which
is ultimately just a functor) to be extracted and passed into another computation.
This explanation may seem obtuse, but consider the same code, with some extracted
whitespace added and the `return` function used:

```haskell
    maybeSomeValue = Just 5 >>= (\x ->
                     Just (x * x) >>= (\x ->
                     return (x + x)))
```

If the reader squints hard enough, this looks like an imperative program! It
looks like the following is being done:

```haskell
    maybeSomeValue = do
        x <- Just 5
        x <- Just (x * x)
        return (x + x)
```

The result of the imperative-looking code is exactly the same as that of the
original computation, if traced through. Using monads to provide an imperative
interface in a functional program is such a common pattern that the `do` notation
was conceived as syntactic sugar to make writing such a pattern easier. That means
that the imperative-looking code is actually valid Haskell!

In addition to `Maybe`, there are several other monads. A major example is the
IO monad, which allows external state to be encapsulated in the monad an interfaced
with in a manner familiar to imperative programmers.

Monads are a big topic, and additional resources are available:

* [Monads](http://learnyouahaskell.com/a-fistful-of-monads)
* [Monad Laws](https://en.wikibooks.org/wiki/Haskell/Understanding_monads#Monad_Laws) that every monad should obey
* [IO Monad](http://learnyouahaskell.com/input-and-output)
* [State Monad](https://en.wikibooks.org/wiki/Haskell/Understanding_monads/State), allows state to be carried in a monadic context, allowing imperative-style computation
* [ST Monad](https://en.wikibooks.org/wiki/Haskell/Mutable_objects), allows mutable state to be carried in a monadic context, useful for implementing inherently destructive algorithms
* [Arrays](https://wiki.haskell.org/Arrays), allows constant-time access to elements like a C array, with mutable variants in the `IO` and `ST` monads provided




# Guides

* [Learn You a Haskell](http://learnyouahaskell.com) is a good beginner text
for learning Haskell. It does not have much real-world examples, but does quite
a good job in explaining difficult theoretical concepts (e.g. functors,
applicative functors and monads) well. It is recommended to read  the material
in 4 chunks:
    * 1-6: basic functional programming
    * 7: modules (very important for large codebases)
    * 8-10: details of the type-system
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

</div>
