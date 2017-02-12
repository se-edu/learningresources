# Introduction to Regular Expressions

Authors: Jamos Tay

# Overview

## What are Regular Expressions?

A Regular Expression (or Regex for short) is a string of characters that define a search pattern. You can think of a regular expression as a list of descriptions that describe a string. These strings can be tested against other text formatted data to find, match and extract data.

Imagine you're a witness of a robbery. How might you describe the robber to the police? You might say that the robber has a beard, brown hair and a facial scar. With this information, the police can round up everyone with beards, brown hair and scars as possible suspects. This is similar to how Regex works. You specify a search pattern using a string (e.g. It starts with 'C', ends with a `S`, and only consists of letters: `C[A-z]+S`) and regex searches a section of text for you, finding all the possible matches.

## Why should I learn Regex?

* **Regex is everywhere** - Regular Expressions are technically a language by themselves, but they are usually implemented within other languages. Almost any commonly used language, like Java, C++ or Python supports Regex is some fashion (A full list is available [here](http://www.regular-expressions.info/tools.html)). Even most decent IDEs or text editors support Regex in 'Find and replace' operations, such as Eclipse, Visual Studio or Notepad++, allowing non-programmers to benefit from Regex.
* **Regex is portable** - People often complain that switching to a new language usually means having to learn a new API and syntax. Regex however is implemented almost the same way on every single language, so you can just learn it once and use it everywhere.
* **Regex is useful** - Although Regex doesn't have a specific use, it is a general use library with many different applications. It can be used automate many different common programming tasks such as validating user input, parsing data from text, and string manipulation, all of which are commonly done in any programming project.
* **Regex is convenient** - Having struggled with many different string handling libraries over the years, I know that string processing is usually a mundane and tedious task. Regex offers a simple and painless way of dealing with them.


# Getting Started

## The basics

The power of regex revolves around the idea that most data comes in a regular, predictable format (e.g. serial numbers, collected data, statistics). Some everyday examples are as follows:

* Phone numbers: +65 65162727
* Passwords: Contains lower/upper case, symbol…
* IP Addresses: 127.0.0.1:8080
* Date time: DD/MM/YY HH:MM

Regex exploits these simlarities and provides a way of specifying patterns that can fit almost any use case. Some of the basic functionalities are as follows:

Name | Regex | Use | Example
Literal characters | Any character(s) | Matches any substring | `cat` matches `category`
Escape characters | `\t`, `\n`, `\d`... | Matches [special characters](http://www.regular-expressions.info/refcharacters.html). | `\t` matches a tab character
Anchors | `^`, `$` | Matches start and end of string | `^ant` matches `antics` but not `pant`
Charcter classes | `[abc]` | Matches any character in the class | `l[ai]st` matches `last` and `list`
Quantifiers | `+` | Matches one or more of the previous character | `no+` matches `nooooooo` but not `n`
 | `?` | Matches zero or one of the previous character | `colou?r` matches `color` and `colour`
 | `*` | Matches zero or more of the previous character | `ba*` matches `b`, `ba` and `baaa`
 | `{1, 3}` | Matches the previous character a specified number of times | `a{1, 3}` matches `a`, `aa` and `aaa`
Groups | `(...)` | Groups a pattern of data | `Used for extracting data, see below


For a full list of possible operators, refer to this quick start guide:
[Quick Start Guide](http://www.regular-expressions.info/quickstart.html)<br>

## Search and Replace

Let's take a look at some simple examples without code. For this guide, you can use any text editor that supports Regex functionality (e.g. [Notepad++](https://notepad-plus-plus.org/download/v7.3.1.html), [Sublimetext](https://www.sublimetext.com/)).

Say you have a passage of text which contains NRICs to be censored.

### Example

> John - S1234567A
> Mary - S8192853B
> David - S1235326C

This can't be done with a simple search and replace, since NRICs have different digits, and we can't specify which ones to look for. However, we know that an NRIC consists of an S, 7 digits from 0-9 and ends with a letter. Knowing this we can construct a Reges:

* Find: `S[\d]{7}[A-Z]`
* Replace: `SXXXXXXXX`

As we can see, the `S` matches the very first S, [\d]{7} matches a string of 7 digits, and [A-Z] matches a single capital letter. We then replace all the critical information with a string of Xs.

### Result

> John - SXXXXXXXX
> Mary - SXXXXXXXX
> David - SXXXXXXXX

## Capturing groups

Now, imagine you have a section of text with many dates written in the US Date Format (MM/DD/YYYY).

> Calendar
> 02/20/2017 Paul's Birthday
> 02/24/2017 Presentation at work
> 02/25/2017 Workshop

However, you want them to be written in (DD/MM/YYYY) instead. As we can see, this isn't as simple as just finding and replacing text, we must also maintain the data in the search string (e.g. `02/20` becomes `20/02`). This can be done using a **capturing group**. A capturing group is a part of a Regex surrounded by brackets `()`. They are numbered `1`, `2`, `3`, from left to right and are used to hold captured data. For example:

`(\d\d)/(\d\d)/(\d\d\d\d)` against `02/20/2017` will result in group 1 : `02`, group 2 : `20`, and group 3 : `2017`.

Using this, we can construct our regex as follows:

* Find: `(\d\d)/(\d\d)/(\d\d\d\d)`
* Replace: `\2/\1/\3`

### Result

> Calendar
> 20/02/2017 Paul's Birthday
> 24/02/2017 Presentation at work
> 25/02/2017 Workshop

## Helpful links

[Try it out online](https://regex101.com/)<br>
[Regex Examples](http://www.regular-expressions.info/examples.html)<br>
[Regex Course](https://regexone.com/)

# Advanced topics

## The Regex Engine

So, how does Regex work exactly?

Regex works similarly to most brute force searches. A simplified Regex engine would look something like this:

1. Start from the start of the text and the start of the regex.
2. Check if the first character matches.
3. If it does check the second character of both strings and so on.
4. If any character doesn't match, start from the second character of the text.
5. Return when a match is found, or if no match is found.

For example, for the regex `light` against the sentence `He likes light lightbulbs.`:

> `He likes light lightbulbs.`:
> `^`
> `light`

First, the engine checks against the start of the string, `H` against `l`. This doesn't match, so the engine moves on to `e`, then ` `, and so on.

> `He likes light lightbulbs.`:
> `   ^`
> `   light`

Once it reaches the first `l`, it sees a match, consumes the `l` and starts comparing the rest of the string.

> `He likes light lightbulbs.`:
> `     ^`
> `   light`

However, when it reaches `k`, it finds a different character so it knows it's not a match. It then starts again from `i` (the letter after `l`).

> `He likes light lightbulbs.`:
> `         ^`
> `         light`

Finally, when it finds a match, it returns the match and terminates. Note that the second `light` in `lightbulbs` is ignored.

Of course, more complex Regex functions will require more steps, but this will give you a rough understanding of the engine for now.

## Greedy Operators

The operators `{}`, `*` and `+` are called greedy operators because they try to match as many characters as possible. For example, for the regex `b.*` against the string `baaaaaa`:

> `baaaaaaaaa`
> ` ^`
> `ba`

Although a suitable match (`ba`) has already been found, the engine sees that it can match against a longer string of `a`s. It will instead return the match `baaaaaaaaa`, matching the entire string.

But what if a greedy operator can't find a match? A greedy operator will always consume as many characters as possible, but if the string that follows fails to match, the engine releases one character at a time until a match is found. This is called 'backtracking'. For example, consider the regex `c.*ed` against the string `contested`.
 
> `contested`
> `        ^`
> `c........ed`

Remember that `.*` matches any number of characters, and this includes the `ed` at the back! The entire string is matched by `c.*`, leaving no characters left for `ed`. This obviously doesn't match, so the engine backtracks one step:

> `contested`
> `        ^`
> `c.......ed`

As we can see, `e` still doesn't match `d`, so we take another step back:

> `contested`
> `       ^`
> `c......ed`

Finally, we get a match and see that both strings match.

## So, what does this mean?

## Helpful links

[The Regex Engine](http://www.regular-expressions.info/engine.html)<br>
[Catastrophic Backtracking](http://www.regular-expressions.info/catastrophic.html)

## When NOT to use Regex


## Reflection

// Reflection overview

## Learning resources

[Cheat sheet for quick reference](https://www.cheatography.com/davechild/cheat-sheets/regular-expressions/pdf/)<br>
[Comprehensive Regex Knowledge Base](http://www.regular-expressions.info/)

### Fun Stuff
[Regex Golf - Test your Regex skills!](https://alf.nu/RegexGolf)<br>
[Regex Crossword - Crosswords, but with Regex](https://regexcrossword.com/)

### Further Reading
Regular Expressions Cookbook - *Jan Goyvaerts, Steven Levithan*
Teach Yourself Regular Expressions in 10 Minutes - *Ben Forta*
Mastering Regular Expressions - *Jeffrey Friedl*