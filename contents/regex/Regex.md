<frontmatter>
  title: Introduction to Regular Expressions
  pageNav: 3
</frontmatter>

<div class="website-content">

# Introduction to Regular Expressions

Authors: Jamos Tay

<box id="article-toc">

* [Overview‎](#overview)
  * [What are Regular Expressions?‎](#what-are-regular-expressions)
  * [Why Should I Learn Regex?‎](#why-should-i-learn-regex)
* [Getting Started‎](#getting-started)
  * [Introduction‎](#introduction)
  * [Search Text‎](#search-text)
    * [Example‎](#example)
  * [Search and Replace‎](#search-and-replace)
    * [Result‎](#result)
  * [Capturing Groups‎](#capturing-groups)
    * [Result‎](#result)
  * [Further Exploration‎](#further-exploration)
  * [Quick Reference‎](#quick-reference)
  * [Helpful Links‎](#helpful-links)
* [Advanced Topics‎](#advanced-topics)
  * [The Regex Engine‎](#the-regex-engine)
  * [Greedy Operators‎](#greedy-operators)
    * [Effect on Performance‎](#effect-on-performance)
    * [Catastrophic Backtracking‎](#catastrophic-backtracking)
    * [Matching Delimiters‎](#matching-delimiters)
  * [Related Links‎](#related-links)
  * [Learning Resources‎](#learning-resources)
    * [Fun Stuff‎](#fun-stuff)
    * [Further Reading‎](#further-reading)
</box>

# Overview

## What are Regular Expressions?

A Regular Expression (or Regex for short) is a string of characters that define a search pattern. You can think of a regular expression as a list of descriptions that describe a string. These strings can be tested against other text formatted data to find, match, and extract data.

Imagine you're a witness of a robbery. How might you describe the robber to the police? You might say that the robber has a beard, brown hair and a facial scar. With this information, the police can round up everyone with beards, brown hair and scars as possible suspects. This is similar to how Regex works. You specify a search pattern using a string (e.g. It starts with `C`, ends with a `S`, and only consists of letters: `C[A-z]+S`) and the section of text is searched, finding all the possible matches.

## Why Should I Learn Regex?

* **Regex is everywhere** - Regular Expressions are technically a language by themselves, but they are usually implemented within other languages. Almost all commonly used languages, like Java, C++ or Python supports Regex is some fashion (A full list is available [here](https://www.regular-expressions.info/tools.html)). In addition, many IDEs or text editors support Regex in 'Find and replace' operations, such as Eclipse, Visual Studio or Notepad++, allowing non-programmers to benefit from Regex.
* **Regex is portable** - People often complain that switching to a new language usually means having to learn a new API and syntax. Regex however is implemented almost the same way on every single language, so you can just learn it once and use it everywhere.
* **Regex is useful** - Although Regex doesn't have a specific use, it is a general use library with many different applications. It can be used automate many different common programming tasks such as validating user input, parsing data from text, and string manipulation, all of which are commonly done in any programming project.
* **Regex is convenient** - "Regex offers a simple and painless way of dealing some with them" because not all string processing are regex-related.


# Getting Started

## Introduction

The power of regex revolves around the idea that most data comes in a regular, predictable format. Some everyday examples are as follows:

* Phone numbers: +65 65162727
* Passwords: Contains lower/upper case, symbol…
* IP Addresses: 127.0.0.1:8080
* Date time: DD/MM/YY HH:MM

Regex exploits these simlarities and provides a way of specifying patterns that can fit almost any use case. Some of the basic functionalities are as follows:

Let's take a look at some simple examples without code. For this article, you can use any text editor that supports Regex functionality (e.g. [Notepad++](https://notepad-plus-plus.org/download/v7.3.1.html), [Sublimetext](https://www.sublimetext.com/)).

## Search Text

The most basic use of Regex is searching a section of text. Say you want to find all occurences of a word in a text. Simple, right? Just use the search function. However, what if you aren't looking for a specific string? For example, if you're looking for phone numbers (represented simply as an 8 digit number) in a chat log, you don't want a specific 8 digit number, you want to find all possible 8 digit numbers. This can't be done with your traditional Ctrl-F search.

Rather, what you're looking for is a **pattern** - a string that satisfies certain properties. Regex allows you to specify these properties using a pattern string, also known as the titular **Regular Expression**. These patterns encapsulate properties about the string you're looking for.

### Example

Let's go back to our example of finding phone numbers in a text. You know that a string representing a phone number must satisfy two criteria: It consists of 8 digits, and may have a country code `65` in front of it. This is what the equivalent pattern for such a string might look like:

`(65)?[\d]{8}`

Firstly, the phone number may start with a country code 65. This criterion is embedded in `(65)?`. The first part, `(65)`, matches the country code. The question mark `?` means that the `(65)` that comes before it is optional.

Secondly, a phone number must have 8 digits. This is specified by `[\d]{8}`. `[\d]` matches a single digit from 0-9, and `{8}` means that `[\d]` is repeated 8 times. Putting them together, `[\d]{8}` matches a string of 8 digits.

As we can see, the pattern `(65)?[\d]{8}` when translated to simple English, means 'Look for a string of 8 numerical digits, that may have an extra `65` in front of it'. This satisfies our criteria for matching a phone number, so we can use it to search our text. Here are some examples of matching strings:

```
6591234567
87775555
67777188
```

## Search and Replace

Note: If you're using one of the recommended text editors to search and replace, please ensure that the search mode is set to 'Regular Expression'. This is usually an option in the 'Find and Replace' (i.e. Ctrl+F) window.

Regex can also be used to search and replace strings.

Say you have a passage of text which contains NRICs to be censored.

> * John - S1234567A
> * Mary - S8192853B
> * David - S1235326C

This can't be done with a simple search and replace, since NRICs have different digits, and we can't specify which ones to look for. However, we know that an NRIC consists of an S, 7 digits from 0-9 and ends with a letter. Knowing this we can construct a Regex:

* Find: `S[\d]{7}[A-Z]`
* Replace: `SXXXXXXXX`

As we can see, the `S` matches the very first S, [\d]{7} matches a string of 7 digits, and [A-Z] matches a single capital letter. We then replace all the critical information with a string of Xs.

### Result

> * John - SXXXXXXXX
> * Mary - SXXXXXXXX
> * David - SXXXXXXXX

## Capturing Groups

Now, imagine you have a section of text with many dates written in the US Date Format (MM-DD-YYYY).

> Calendar
> * 02-20-2017 Paul's Birthday
> * 02-24-2017 Presentation at work
> * 02-25-2017 Workshop

However, you want them to be written in (DD-MM-YYYY) instead. As we can see, this isn't as simple as just finding and replacing text, we must also maintain the data in the search string (e.g. `02-20` becomes `20-02`). This can be done using a **capturing group**. A capturing group is a part of a Regex surrounded by brackets `()`. They are numbered `1`, `2`, `3`, from left to right and are used to hold captured data. For example:

`(\d\d)-(\d\d)-(\d\d\d\d)` against `02/20/2017` will result in group 1 : `02`, group 2 : `20`, and group 3 : `2017`.

Using this, we can construct our regex as follows:

* Find: `(\d\d)-(\d\d)-(\d\d\d\d)`
* Replace: `\2-\1-\3`

In the replace field, `\1` represents the group matched in the string. For example, `\1` corresponds to `02`, `\2` corresponds to `20` and so on. As you can see, we are swapping group 1 (the month) and group 2 (the day) to get the desired result.

### Result

> Calendar <br>
> * 20-02-2017 Paul's Birthday <br>
> * 24-02-2017 Presentation at work <br>
> * 25-02-2017 Workshop

## Further Exploration

These examples provide a quick look as to what Regex can be used for, without code. You can also use Regex as part of many different programming languages, to search strings within the code. The implementation of Regex varies from language to language (for example, Python uses the [`re`](https://docs.python.org/2/library/re.html) library, Java uses [`Pattern`](https://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html) and Javascript has regex built into the language using [`/` notation](https://developer.mozilla.org/en/docs/Web/JavaScript/Guide/Regular_Expressions)), but the functionality is consistent so it's not difficult to port Regex from one language to another. You can search the respective language documentation for a more comprehensive view of how to use Regex in the language of your choice.

For more examples of what Regex can do, you can refer to the tutorial links in the appendix.

## Quick Reference

Name | Regex | Use | Example
--- | --- | --- | ---
Literal characters | Any character(s) | Matches any substring | `cat` matches `category`
Escape characters | `\t`, `\n`, `\d`... | Matches [special characters](https://www.regular-expressions.info/refcharacters.html). | `\t` matches a tab character
Anchors | `^`, `$` | Matches start and end of string | `^ant` matches `antics` but not `pant`
Character classes | `[abc]` | Matches any character in the character class | `l[ai]st` matches `last` and `list`
Quantifiers | `+` | Matches one or more of the previous character | `no+` matches `nooooooo` but not `n`
 | `?` | Matches zero or one of the previous character | `colou?r` matches `color` and `colour`
 | `*` | Matches zero or more of the previous character | `ba*` matches `b`, `ba` and `baaa`
 | `{1, 3}` or `{2}` | Matches the previous character a specified number of times | `a{1, 3}` matches `a`, `aa` and `aaa`, `a{2}` matches `aa`
Groups | `(...)` | Groups a pattern of data | `Used for extracting data, see above`


For a full list of possible operators, refer to this cheat sheet:
[Regex cheat sheet](https://www.rexegg.com/regex-quickstart.html)<br>

## Helpful Links

* [Regex101](https://regex101.com/) - An online Regex engine that allows the user to test Regexes against text and walks the user through their execution.
* [Regular-Expressions](https://www.regular-expressions.info/examples.html) - Some common examples of where Regex can be used, along with pre-compiled Regexes that can be useful.
* [Regexone](https://regexone.com/) - A free online course that teaches the many features of Regex interactively.
* [Rexegg](https://www.rexegg.com) - A comprehensive online tutorial for Regex that documents many advanced functionalities of Regex.

# Advanced Topics

## The Regex Engine

So, how exactly does Regex work?

Regex works similarly to most brute force searches. A simplified Regex engine would look something like this:

1. Start from the start of the text and the start of the regex.
1. Check if the first character matches.
1. If it does, check the second character of both strings.
1. If it does, check the third character of both strings, and so on.
1. If any character doesn't match, start from the second character of the text.
1. Return when a match is found, or if no match is found.

For example, for the regex `light` against the sentence `He likes light lightbulbs.`:

```
He likes light lightbulbs.
^
light
```

First, the engine checks against the start of the string, `H` against `l`. This doesn't match, so the engine moves on to `e`, then ` `, and so on.

```
He likes light lightbulbs.
   ^
   light
```

Once it reaches the first `l`, it sees a match, consumes the `l` and starts comparing the rest of the string.

```
He likes light lightbulbs.
     ^
   light
```

However, when it reaches `k`, it finds a different character so it knows it's not a match. It then starts again from `i` (the letter after `l`).

```
He likes light lightbulbs.
         ^
         light
```

Finally, when it finds a match, it returns the match and terminates. Note that the second `light` in `lightbulbs` is ignored.

Of course, more complex Regex functions will require more steps, but this will give you a rough understanding of the engine for now.

## Greedy Operators

The operators `{}`, `*` and `+` are called greedy operators because they try to match as many characters as possible. For example, for the regex `b.*` against the string `baaaaaa`:

```
baaaaaaaaa
 ^
ba
```

Although a suitable match (`ba`) has already been found, the engine sees that it can match against a longer string of `a`s. It will instead return the match `baaaaaaaaa`, matching the entire string.

But what if a greedy operator can't find a match? A greedy operator will always consume as many characters as possible, but if the string that follows fails to match, the engine releases one character at a time until a match is found. This is called 'backtracking'. For example, consider the regex `c.*ed` against the string `contested` (Let `.....` represent `.+`).

```
contested
        ^
c........ed
```

Remember that `.*` matches any number of characters, and this includes the `ed` at the back! The entire string is matched by `c.*`, leaving no characters left for `ed`. This obviously doesn't match, so the engine backtracks one step:

```
contested
        ^
c.......ed
```

As we can see, `e` still doesn't match `d`, so we take another step back:

```
contested
       ^
c......ed
```

Finally, we get a match and see that both strings match.

### Effect on Performance

Bad use of greedy operators (`.*`, `.+`) can actually lead to an unwanted increase in performance time. Because they scan the entire string, you may end up processing more text than necessary. Consider the following scenario:

Match a person's full name: `(Anderson.*Cooper)`  <br>
Text: `Hi, my name is Anderson Pearson Cooper and I am a programmer from ... (3000 words redacted)`

This looks like a simple matching at first glance, but look at what happens:

```
Hi, my name is Anderson Pearson Cooper and I am a programmer from ... (3000 words redacted)
               ^
               Anderson..............................................
```

Every character after `Anderson` matches `.*`, resulting in scanning the entire string! And not only that, the Regex also has to backtrack one character at a time, until it reaches `Anderson Pearson ` and `Cooper` can be matched. This results in an O(String length) complexity even for the best case, which is bad considering we can write a brute force search that terminates after O(Regex length) in the best case.

### Catastrophic Backtracking

This is a famous pitfall in Regular Expressions, so much so it deserves its own name and book section. Catastrophic backtracking usually occurs when multiple greedy operators are chained. If a match can't be found, each operator will be backtracked individually, which results in a lot of computation, even up to O(2<sup>n</sup>) in some cases! Let's look at one case in detail.

### Matching Delimiters

One common reason this happens is when delimited data is carelessly matched. This is when the Regex group you are using to match the data also matches the delimiter. Consider the following example:

Match 10 comma separated values in an array: `\[(.+,){9}(.+)\]`<br>
E.g.: `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`

This regex looks innocent enough. It matches 9 groups of data with a comma, followed by one group without, so what's wrong?

The problem occurs because `.` matches any character. This also includes the comma separating the group. This is what happens when the first set of `(.+,)` is parsed:

```
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
^
[.........................,
[.+                       ,
```

The first group keeps everything as its data group. Now, it tries to find the second `(.+,)` in ` 10]`, but fails, so it backtracks:

```
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
^
[......................,..,
[.+                    ,.+,
```

This time the first and second groups are matched. Now, it tries to find the **third** `(.+,)` in ` 10]`, but fails, so it backtracks yet again. So we move the first `(.+,)` back:

```
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
^
[...................,
[.+                 ,
```

Now, what happens? Well, the second `(.+,)` is greedy, so it actually matches the last comma instead!

```
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
^
[...................,.....,
[.+                 ,.+   ,
```

As we can see, by chaining repeated groups, it actually causes cascading levels of backtracking, which leads to very poor performance, even with short strings. For this case, we can observe that each comma is either included in the regex or not included in the match, and every possible combination of included and not included combinations will appear at least once. That means the complexity is equal to the number of possible ways n commas can be either included or not included, or O(2<sup>n</sup>). This is a Very Bad Thing™!

This problem can be exacerbated by the one mentioned previously. Imagine if your text looked like this:

```
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10] some unrelated words, blah blah (200 more redacted)
[...................................................,
```

The extra parts of the string will be matched each time, and we can only imagine how long that will take.

So, how do we prevent this?

* Explicitly exclude the delimter.

> Match 10 comma separated values in an array: `\[([^,]+,){9}([^,]+)\]` instead of `\[(.+,){9}(.+)\]`<br>

Here, instead of matching all characters (`.`), we match all characters except a certain character (`[^,]`). This allows the regex to capture in one parse successfully. Some other common examples are HTML/XML tags (`<[^>]*>` instead of `<.+>`) and bracketed expressions (`\([^\)]+\)` instead of `\(.+\)`).

* Always narrow the scope as much as possible.

> Matching a name: `[A-z]+ [A-z]+` instead of `.+ .+`<br>

A great tip for improving Regex performance is to specify your match as clearly as possible. This reduces the potential of unwanted matchings and unnecessary backtracking.

* Avoid greedy operators if possible.

> Matching a date: `\d{1,2}-\d{1,2}-\d{2,4}` instead of `.*-.*-.*`<br>

If you know your data will only be a few characters long, try your best to avoid using `*` or `+`. Limiting to a number of characters would reduce the amount of backtracking if any.

* Use Lazy Quantifiers

> Use `.+?` or `.*?` instead of `.+` and `.*`

A greedy quantifier `+` or `*` can be turned into a lazy quantifier by adding an extra `?` behind it, i.e. (`.+?` or `.*?` instead of `.+` and `.*`). A lazy quantifier works similarly to a greedy quantifier, but it stops as soon as it finds a match rather than matching as much as possible. For instance, in our `contested` example, if we replaced `c.+d` with `c.+?d`, the engine would stop matching the `.+` here:

```
contested
  ^
c.ed
```

Since `o` is a suitable match for `.+`, the engine accepts it and tries to match the `ed`. If no match is found, the engine would bactrack and match `c..ed`, and so on. We can see a clear advantage here for matching short strings, as we can guarantee that we'll never iterate further than the length of the pattern.


## Related Links

* [The Regex Engine](https://www.regular-expressions.info/engine.html)
* [Catastrophic Backtracking](https://www.regular-expressions.info/catastrophic.html)

## Learning Resources

* [Printable cheat sheet for quick reference](https://www.cheatography.com/davechild/cheat-sheets/regular-expressions/pdf/)
* [Comprehensive Regex Knowledge Base](https://www.regular-expressions.info/)

### Fun Stuff
* [Regex Golf - Test your Regex skills!](https://alf.nu/RegexGolf)
* [Regex Crossword - Crosswords, but with Regex](https://regexcrossword.com/)

### Further Reading
* [Regular Expressions Cookbook](https://www.amazon.com/Regular-Expressions-Cookbook-Solutions-Programming/dp/1449319432) - *Jan Goyvaerts, Steven Levithan*
* [Teach Yourself Regular Expressions in 10 Minutes](https://www.amazon.com/exec/obidos/ASIN/0672325667/jgsbookselection) - *Ben Forta*
* [Mastering Regular Expressions](https://www.amazon.com/Mastering-Regular-Expressions-Jeffrey-Friedl/dp/0596528124) - *Jeffrey Friedl*
</div>
