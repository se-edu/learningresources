# Swift

Authors: Ch'ng Ming Shin

# Overview

Introduced in 2014, Swift is a fast, modern, safe and interactive programming language. If you have ever thought of building an app for your MacBook or iPhone (or any other Apple Product), Swift is the language for you. With its syntactic sugar, it is much more readable compared to it's "predecessor" Objective-C(ObjC). Together with its Playground feature, you can run your code easily, making Swift easy to learn.

<!--
If you have decided to build an app for your MacBook or iPhone (or any other Apple Product) or transitioning from another language to Swift, I am here to make your life easier. The common solutions to "How to learn Swift" from Quora and StackOverflow are to

1. Dive into a project

2. Read the official documentation
https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/index.html


Those are great suggestions but diving into a new project with no prior experience to Swift makes it an ardous trial of google searches of how to implement stuff. And reading documentation, while really helpful, might be overkill and boring as there might be many concepts that you already know.

To ease this pain, I am providing a 3rd solution where I try to summarise the more common and important aspects of Swift that are generally different from other languages.
-->

# Getting Started

## Good readings on Swift:
- Design of Swift, why the change from obj-C:
http://swiftjester.org/papers/whats-different-about-swift.html


- Main Swift Concepts (less indepth, wide range of topics, long explanations):
https://guides.codepath.com/ios/Swift-Basics

- Basics of Apple Swift in playground:
https://github.com/Codeido/swift-basics

- Guide to Xcode(IDE for Swift):
http://codewithchris.com/xcode-tutorial/

- Another compilation of links:
http://www.whoishostingthis.com/resources/swift/

## Cheatsheets:
- One page cheatsheet:
https://koenig-media.raywenderlich.com/uploads/2014/06/RW-Swift-Cheatsheet-0_7.pdf

- Multiple pages of cheatsheet (mainly code examples with comments):
https://mhm5000.gitbooks.io/swift-cheat-sheet/content/index.html




# Swift Basics

## [The Absolute Basics](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/TheBasics.html)

Things to note:

Constant and variable names can contain almost any character, including Unicode characters

There is the idea of mutability of variables. If a variable should not be immutable, use let to declare it as a immutable constant. (let is similiar to final in Java)

```
let a = 2
var b = 3
```

Unlike Java, it is not compulsory to annotate the type of a variable, but once the type of a variable is decided, it cannot be changed.

```
var c: Int
var d //this is legal
```

nil in Swift is the same as null in Java

There will be more readings on Error-Handling and Optionals

## [Basic Operators](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/BasicOperators.html)

Things to note:

- Range Operators (Can be used to access arrays, mostly used in for-loops)

  Swift includes two range operators, which are shortcuts for expressing a range of values.

  - Closed Range Operator

    The closed range operator (a...b) defines a range that runs from a to b, and includes the values a and b. The value of a must not be greater than b.

  - Half-Open Range Operator

   The half-open range operator (a..<b) defines a range that runs from a to b, but does not include b. It is said to be half-open because it contains its first value, but not its final value. As with the closed range operator, the value of a must not be greater than b. If the value of a is equal to b, then the resulting range will be empty.

## [Strings and Characters](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/StringsAndCharacters.html#//apple_ref/doc/uid/TP40014097-CH7-ID285)

Things to note:

- The escaped special characters \0 (null character), \\ (backslash), \t (horizontal tab), \n (line feed), \r (carriage return), \" (double quote) and \' (single quote).

- You can use + to concatenate Strings and use indexes directly on Strings.

  ```
  var str1 = "This is the first part "
  var str2 = "of this sentence.\n"
  var str3 = str1 + str2 //"This is the first part of this sentence.\n"
  var tenthChar = str3[9] //'h'
  ```

## [Collection Types](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/CollectionTypes.html)

Things to note:

- A dictionary is similar to HashMap in other languages.


Other references:

- Lesser-known Foundation Data Structures: https://www.raywenderlich.com/123100/collection-data-structures-swift-2

  If a dictionary, array or set won’t do the job, it’s worth checking if one of these will work before you create something from scratch.


## [Control Flow](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ControlFlow.html)

Things to note:

- For loop example:<br>
Instead of `for(int i = 0; i<100; i++) {` in Java, you would do `for i in 0..<100 {`.

- Repeat-While is the equivalent of Do-While in other languages.

- Switch does not require an explicit break for each case as switch statements in Swift do not fall through the bottom of each case and into the next one by default.

  ```
  let integerToDescribe = 5
  var description = "The number \(integerToDescribe) is"
  switch integerToDescribe {
  case 2, 3, 5, 7, 11, 13, 17, 19:
      description += " a prime number, and also"
      fallthrough
  default:
      description += " an integer."
  }
  print(description)
  // Prints "The number 5 is a prime number, and also an integer."
  ```

  There are many other subtle differences in switch-case for Swift, such as interval matching, testing multiple values in one statement using tuples, able to bind values in switch-cases to constants and variables, and using `where` to check for additional conditions.

- The term `guard` is similar to an `if` statement, but it always has an `else` clause. It is generally used in the case of error handling, to guard against invalid(`nil`) variables that are used in the later part of the code. When the `guard` statement test fails, the `else` clause is used to exit the current method or loop, so that the invalid variable is not accessed. This will be covered more in Error-Handling.

## [Functions](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Functions.html)

Things to note:

- An example of a function:

  ```
  func greet(person: String, alreadyGreeted: Bool) -> String {
      if alreadyGreeted {
          return greetAgain(person: person)
      } else {
          return greet(person: person)
      }
  }
  ```

- In the past(Swift 2), the label for the first parameter of a function is implicitly omitted when you call the function. To find out why, as well as learn more about parameter labels and naming functions well: http://inaka.net/blog/2016/09/16/function-naming-in-swift-3/


- Function return types can be optionals.

- Functions are special cases of closure, both are reference types.

- Functions are [first class citizens](http://www.figure.ink/blog/2016/9/3/first-class-functions-in-swift); [this means you can](http://stackoverflow.com/questions/29022985/how-to-comprehend-the-first-class-function-in-swift) :
  - assign a function to a local variable,
  - pass a function as an argument to another function, and
  - return a function from a function.
- In-out parameters(defined by the keyword `inout`) allow changes to parameters to persist after the function call ends. Since the parameter can be modified, only variables can be passed. An ampersand(&) must be placed before the argument to indicate that it can be modified by the function.

  ```
  var a = 10
  func doSthg(inout num) {
    num += 1
  }
  doSthg(a)
  print(a) //a is now 11
  ```

Cheatsheet for functions(less wordy than the docs):
http://limlab.io/swift/2016/02/12/swift-functions-cheatsheet.html


## [Closures](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html)

### Introduction to Closures
Closures are similar to lambdas in other languages. They can be written in a lightweight syntax that can capture values from their surrounding context.

### Closure Optimizations
Swift’s closure expressions have a clean, clear style, with optimizations that encourage brief, clutter-free syntax in common scenarios. These optimizations include:
- Inferring parameter and return value types from context
- Implicit returns from single-expression closures
- Shorthand argument names
- Trailing closure syntax

Consider the following example for sorting an array in reverse.
We first use a function:
```
func backward(_ s1: String, _ s2: String) -> Bool {
    return s1 > s2
}
var reversedNames = names.sorted(by: backward)
```

Now we use a closure:
```
reversedNames = names.sorted(by: {
  (s1: String, s2: String) -> Bool in
    return s1 > s2
})
```
In this case, you can think of a closure as a function with no name. Now, lets look at how we can improve the syntax of closures.

#### Inferring Type From Context:
Omit types for closure input.
```
reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )
```

#### Implicit returns from single-expression closures:
Omit return
```
reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )
```

#### Shorthand argument names:
Omit argument list, use $0 and $1 refer to the closure’s first and second String arguments.
```
reversedNames = names.sorted(by: { $0 > $1 } )
```

#### Trailing closure syntax:
Trailing closures are most useful when the closure is sufficiently long that it is not possible to write it inline on a single line.  
```
reversedNames = names.sorted() { $0 > $1 }
```
If the closure expression is the only argument, you can also discard the () following the function’s name
```
reversedNames = names.sorted { $0 > $1 }
```
Isn't it much neater now?

### Escaping and Nonescaping Closures in Swift 3:
A non-escaping closure means that the closure that is passed as an argument to a function will be invoked before the function returns.

On the other hand, if a closure is passed as an argument to a function and it is invoked after the function returns, the closure is escaping.

By default, closures are non-escaping in Swift 3. If the compiler knows that a closure is non-escaping, it can take care of a many of the nitty-gritty details of memory management.

More in-depth explanations and examples:
https://swiftunboxed.com/lang/closures-escaping-noescape-swift3/
https://cocoacasts.com/what-do-escaping-and-noescaping-mean-in-swift-3/

### Autoclosures
You are unlikely to need to use autoclosures, but if you're curious, or if you ever see the term `@autoclosure`:
An autoclosure is a closure that is automatically created to wrap an expression that’s being passed as an argument to a function.

Examples of autoclosures:
https://cocoacasts.com/how-to-use-autoclosures-and-autoclosure-in-swift-3/

Quick cheatsheet of functions and closures: http://fuckingswiftblocksyntax.com/

In-depth cheatsheet of closures:
http://limlab.io/swift/2016/02/13/swift-closures-cheatsheet.html


# Others
Once you start to get used to Swift and expand your project, you might come across new terms.

- Difference between Cocoa and Cocoa Touch

  Quoted from a stackoverflow answer comment:
  "Cocoa is for Mac development; Cocoa Touch is for iOS development. If something is only in Cocoa, you can't use it on iOS, and if something is only in Cocoa Touch, you can't use it on Mac OS X."

  http://stackoverflow.com/questions/2297841/cocoa-versus-cocoa-touch-what-is-the-difference
  http://www.whoishostingthis.com/resources/cocoa/


- CocoaPods
  If you are the kind of person that prefers to do everything yourself, this might be irrelevant to you. Otherwise, CocoaPods is the package manager for Swift. You are likely to use this if you depend on libraries a lot.

  - Getting Started and more information<br>
  https://blog.versioneye.com/2014/01/15/which-programming-language-has-the-best-package-manager/<br>
  https://guides.cocoapods.org/using/getting-started.html
  https://dzone.com/articles/concise-introduction-cocoapods

  - How to use<br>
  https://guides.cocoapods.org/using/using-cocoapods.html

<!--
# Beyond the Basics

Enum
Classes and Structs
Overview of enums-structs-and-classes-in-swift:https://www.raywenderlich.com/119881/enums-structs-and-classes-in-swift
Difference between classes and Structs
When to use classes and Structs

Reference-value types:
https://www.raywenderlich.com/112027/reference-value-types-in-swift-part-1
https://www.raywenderlich.com/112029/reference-value-types-in-swift-part-2

# Advanced topics

## Error Handling
In Swift, instead of try and except,
http://swiftjester.org/papers/understanding-error-handling-in-swift2.html
Swift 3 error handling: https://www.raywenderlich.com/130197/magical-error-handling-swift

## Automatic Reference Counting
This is where you see what is strong and weak. Essentially, you want to prevent strong reference cycles so that there are no memory leaks.
https://krakendev.io/blog/weak-and-unowned-references-in-swift
https://www.andrewcbancroft.com/2015/05/08/strong-weak-and-unowned-sorting-out-arc-and-swift/
-->
