# Swift

Authors: [Ch'ng Ming Shin](https://github.com/ablyx/cs3281-website/blob/mingshin-week6-progress/students/AY1617S2/ch'ngMingShin/Ch'ngMingShin-Resume.md)

# Overview

Welcome to Swift, THE preferred language to do iOS programming. Introduced in 2014, Swift is a fast, modern and safe programming language which supports playgrounds, an innovative feature that allows programmers to experiment with Swift code and see the results immediately, without the overhead of building and running an app.

# Getting Started

Reading the [Language Guide in the official documentation](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/) is definitely recommended, since it explains everything quite clearly, albeit quite verbose.

If you're just looking to take a dip into Swift, here are the [basics](https://guides.codepath.com/ios/Swift-Basics) you'll need to understand.

And if you are really pressed for time, here's a couple of cheatsheets with code examples:

- [Swift Cheat Sheet](https://mhm5000.gitbooks.io/swift-cheat-sheet/content/index.html) (web-friendly)
- [Swift 3.0 Cheat Sheet and Quick Reference](https://koenig-media.raywenderlich.com/uploads/2014/06/RW-Swift-Cheatsheet-0_7.pdf) (print-friendly)

# Cool Features

Here are some cool features that you might not be familiar with but would definitely be useful to you if you are new to Swift. 

## Type Inference

Swift supports type inference whereby the compiler automatically deduces the type of a variable during compilation by examining the values assigned to it.

```swift
let str1: String = "foo"
let str2 = "foo" // compiler infers that str2 is of type String
```

## Optionals

Swift allows you the use of Optionals, so that you can choose to either return nil or a data value, instead of returning a special value to indicate the absence of a value.

```swift
func yearAlbumReleased(name: String) -> Int {
    switch name {
    case "Taylor Swift": return 2006
    case "Fearless": return 2008
    case "Speak Now": return 2010
    case "Red": return 2012
    case "1989": return 2014
    default:
        return -1
    }
}
```
Without Optionals, you might consider using -1 to indicate that there was no such album. But if someone else uses this function, how does he know that -1 means "no such album"? Wouldn't it be better if we could return nil?

```swift
func yearAlbumReleased(name: String) -> Int? {
    switch name {
    case "Taylor Swift": return 2006
    case "Fearless": return 2008
    case "Speak Now": return 2010
    case "Red": return 2012
    case "1989": return 2014
    default:
        return nil
    }
}
```

### Optional Binding

And here is how you unwrap the Optional safely:

``` swift
func timeTravel(album: String) {
    let year = yearAlbumReleased(album) // type of year: Int?
    if let past = year {
        // past is a normal (non-optional) Int value
        // equal to the value stored in year
        travelTo(past)
    } else {
        // year did not hold a value
        liveInPresent()
    }
}
```

To learn more about Optionals, such as Optional Chaining and "dangerously" Force Unwrapping, check out this [article](https://hackernoon.com/swift-optionals-explained-simply-e109a4297298).

If you would like to seek a second (or more) opinion about Optionals, check out this [StackOverflow answer](http://stackoverflow.com/questions/24003642/what-is-an-optional-value-in-swift).

## Structs

Apart from the classes (something you are familiar with if you have already learned languages like Java / Python) which you use for creating instances of Reference type, Swift also provides the use of Structs to create instances of Value type. 

Let's use a simple example (from [Apple's own blog on Swift](https://developer.apple.com/swift/blog/?id=10)) to illustrate the difference between Reference types (Classes) and Value types (Structs). 

```swift
// Value type example
struct S { var data: Int = -1 }
var a = S()
var b = a                       // a is copied to b
a.data = 42                     // changes a, not b
print("\(a.data), \(b.data)")   // prints "42, -1\n"

// Reference type example
class C { var data: Int = -1 }
var x = C()
var y = x                       // x is copied to y
x.data = 42                     // changes the instance referred to by x (and y)
print("\(x.data), \(y.data)")   // prints "42, 42\n"
```

You can think of Structs as a way to create instances that have their own unique copies of data, which can help to make things a lot easier.

If you wish to find out more, here is an [article](https://medium.com/capital-one-developers/reference-and-value-types-in-swift-de792db330b2) that explains the difference between the 2 types, as well as the benefits of value types and when to use them.

## Protocol Oriented Programming

The heart of Swift is Protocol Oriented Programming (POP) which is about abstraction and simplicity. POP helps to solve the [bloat that is sometimes caused by Object Oriented Programming (OOP)](http://blogs.perl.org/users/sid_burn/2014/03/inheritance-is-bad-code-reuse-part-1.html). If you ever find yourself having to inherit from multiple classes, you probably should consider using protocols instead.

Here's some code to serve as a brief introduction to POP:

First, we first introduce our protocols.

```swift
protocol Bird {
  var name: String { get }
  var canFly: Bool { get }
}
 
protocol Flyable {
  var airspeedVelocity: Double { get }
}
```

Next, we introduce the structs that conform to the protocols above.

```swift
// Penguins can't fly ):
struct Penguin: Bird {
    let name: String
    let canFly = false
}

struct Eagle: Bird, Flyable {
    let name: String
    let canFly = true
 
    var airspeedVelocity: Double {
        return 160.0
  }
}
```

And if you haven't noticed, protocols are extremely similar to interfaces in Java. 

To understand more about POP, watching this [WWDC 2015 talk](https://www.youtube.com/watch?v=g2LwFZatfTI) is highly recommended.

## Automatic Reference Counting

A few keywords unique to Swift are `strong`, `weak` and `unowned`, which have to do with Swift's way of memory management, [Automatic Reference Counting (ARC)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AutomaticReferenceCounting.html). 

Essentially, when an instance of a class is created, a chunk of memory is allocated to it until it is no longer strongly referenced by anything. References are strong by default. Thus, if we have an Object A (a `UIViewController`) that creates an Object B (a `UIAlertController`), B would be strongly referenced by A. However, B might also need access to a variable in A, such that A may be strongly referenced by B, resulting in a reference cycle.

Reference cycles are bad, because they cause memory leaks. Even though A and B are no longer needed eventually, A and B will still sit in memory since they are both strongly referenced by each other. This is why we need the `strong`, `weak` and `unowned` keywords, to resolve reference cycles. 

Here is an [article](https://krakendev.io/blog/weak-and-unowned-references-in-swift) with greater in-depth explanation and examples.

## CocoaPods

When you have been working on a Swift project for a while and start to think "Hmm... Someone has probably done something like this before" or "This is a common problem and there should be a library for this", check out [CocoaPods](https://guides.cocoapods.org/using/getting-started.html), THE package/library manager for Swift.