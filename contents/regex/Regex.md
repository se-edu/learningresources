# Introduction to Regular Expressions

Authors: Jamos Tay

# Overview

## What are Regular Expressions?

A Regular Expression (or Regex for short) is a string of characters that define a search pattern. You can think of a regular expression as a list of descriptions that describe a string. These strings can be tested against other text formatted data to find, match and extract data.

Think about a witness of a robbery. How might he describe the robber to the police? He might say that the robber has a beard and brown hair. With this information, the police can round up everyone with beards and brown hair as possible suspects. This is similar to how Regex works. You specify a search pattern using a string (e.g. It starts with 'C', ends with a `S`, and only consists of letters: `[C[A-z]+S]`) and regex searches a section of text for you, finding all the possible matches.

## Why should I learn Regex?

* **Regex is everywhere** - Regular Expressions are technically a language by themselves, but they are usually implemented within other languages. Almost any commonly used language, like Java, C++ or Python supports Regex is some fashion (A full list is available [here](http://www.regular-expressions.info/tools.html)). Even most decent IDEs or text editors support Regex in 'Find and replace' operations, such as Eclipse, Visual Studio or Notepad++, allowing non-programmers to benefit from Regex.
* **Regex is portable** - People often complain that switching to a new language usually means having to learn a new API and syntax. Regex however is implemented almost the same way on every single language, so you can just learn it once and use it everywhere.
* **Regex is useful** - Although Regex doesn't have a specific use, it is a general use library with many different applications. It can be used automate many different common programming tasks such as validating user input, parsing data from text, and string manipulation, all of which are commonly done in any programming project.
* **Regex is convenient** - Having struggled with many different string handling libraries over the years, I know that string processing is usually a mundane and tedious task. Regex offers a simple and painless way of dealing with them.


# Getting Started

## Helpful links

[Try it out online](https://regex101.com/)
[Quick Start Guide](http://www.regular-expressions.info/quickstart.html)
[Regex Examples](http://www.regular-expressions.info/examples.html)
[Regex Course](https://regexone.com/)

# Advanced topics

[The Regex Engine](http://www.regular-expressions.info/engine.html)
[Catastrophic Backtracking](http://www.regular-expressions.info/catastrophic.html)

## Reflection

// Reflection overview

## Learning resources

[Cheat sheet for quick reference](https://www.cheatography.com/davechild/cheat-sheets/regular-expressions/pdf/)
[Comprehensive Regex Knowledge Base](http://www.regular-expressions.info/)

### Fun Stuff
[Regex Golf - Test your Regex skills!](https://alf.nu/RegexGolf)
[Regex Crossword - Crosswords, but with Regex](https://regexcrossword.com/)

### Further Reading
Regular Expressions Cookbook - *Jan Goyvaerts, Steven Levithan*
Teach Yourself Regular Expressions in 10 Minutes - *Ben Forta*
Mastering Regular Expressions - *Jeffrey Friedl*