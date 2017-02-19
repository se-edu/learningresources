# Introduction to Haskell

Authors: Thenaesh Elango


# Overview

Haskell is a purely functional programming language with strong, static,
inferred typing. It is one of the languages that feature the
[second-order lambda calculus](https://en.wikipedia.org/wiki/System_F)
(the other being the ML family of languages), which lends the language much of
its expressive power.

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

### System-Wide Installation

For new users, Haskell may be quickly and easily installed by downloading the
[Haskell Platform](https://www.haskell.org/platform/) for their respective
operating systems. The Haskell Platform contains many common and important
Haskell libraries, in addition to the Glasgow Haskell Compiler (GHC).

At the time of writing, the Haskell Platform has binaries available for the
following operating systems:

* Linux
  * Debian
  * Ubuntu
  * Mint
  * Red Hat
  * Fedora
  * Gentoo
  * a generic binary package
* MacOS
  * Homebrew
  * ordinary installer
* Windows

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

**The rest of this tutorial assumes a _system-wide installation_ of the Haskell
Platform.** If using Stack, the commands will need to be modified accordingly.

## Usage

The command `ghci` may be used to invoke the GHC interpreter. This launches an
[REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) where
Haskell code can be entered and evaluated interactively. This is a very useful
tool when first learning Haskell, and also when debugging code that fails to
compile.

The command `ghc` may be used to compile Haskell code down to machine code. The
invocation of `ghc` is very similar to that of `gcc`. It is preferred to use
Stack for build management in production software, rather than handling
compilation in this manner.


# Whirlwind Tour

## Types

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
