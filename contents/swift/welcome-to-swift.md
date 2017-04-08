# Swift

Authors: [Ch'ng Ming Shin](https://github.com/ablyx/cs3281-website/blob/mingshin-week6-progress/students/AY1617S2/ch'ngMingShin/Ch'ngMingShin-Resume.md)

# Overview

Welcome to Swift, THE preferred language to do iOS Programming. Along with the awesome syntactic sugar and the playground which allows you to play and view results of your code, Here are some cool features that you might not be familiar with but would defnitely be useful to you if you are new to Swift. 

# Cool Features


## Optionals

Swift allows you the use of Optionals, so that you can choose to either return nil or a data type, instead of having to handle for special cases. 

```
func yearAlbumReleased(name: String) -> Int {
    if name == "Taylor Swift" { return 2006 }
    if name == "Fearless" { return 2008 }
    if name == "Speak Now" { return 2010 }
    if name == "Red" { return 2012 }
    if name == "1989" { return 2014 }

    return -1 
}
```
Without Optionals, you might consider using -1 to indicate that there was no such album. But if someone else uses this function, how does he know that -1 means "no such album"? . Wouldn't it be better if we could return nil?

```
func yearAlbumReleased(name: String) -> Int? {
    if name == "Taylor Swift" { return 2006 }
    if name == "Fearless" { return 2008 }
    if name == "Speak Now" { return 2010 }
    if name == "Red" { return 2012 }
    if name == "1989" { return 2014 }

    return nil 
}
```

And here is how you unwrap the Optional safely

```
func timeTravel(album: String) {
    Int? year = yearAlbumReleased(album)
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

To learn more about Optionals, such as how to "dangerously" force unwrap or how to chain a bunch of them, check out
https://hackernoon.com/swift-optionals-explained-simply-e109a4297298

If you would like to seek a second (or many other more opinions), here are several explainations of Optionals
http://stackoverflow.com/questions/24003642/what-is-an-optional-value-in-swift


## Structs

Apart from the classes (something you are familiar with if you have already learned languages like Java / Python) which you use for creating instances of Reference type, Swift also provides the use of Structs to create instances of Value type. 

Let's use a simple example (from [Apple's own blog on Swift](https://developer.apple.com/swift/blog/?id=10)) to illustrate the difference between Reference types (Classes) and Value types (Structs). 

```
// Value type example
struct S { var data: Int = -1 }
var a = S()
var b = a                       // a is copied to b
a.data = 42                     // Changes a, not b
println("\(a.data), \(b.data)") // prints "42, -1"

// Reference type example
class C { var data: Int = -1 }
var x = C()
var y = x                       // x is copied to y
x.data = 42                     // changes the instance referred to by x (and y)
println("\(x.data), \(y.data)") // prints "42, 42"
```

You can think of Structs as a way to create instances that are immutable since no other part of your code can change it, which can help to make things a lot easier.

If you wish to find out more but pressed for time, here is an [article](https://cocoacasts.com/value-types-and-reference-types-in-swift/) that sumamrises the difference between the 2 types, as well as the benefits of value types and when to use them  

Read [this](https://medium.com/capital-one-developers/reference-and-value-types-in-swift-de792db330b2) if you are looking for something more verbose which explains the difference between the 2 types with diagrams.


## Protocol Oriented Programming

The heart of Swift is Protocol Oriented Programming(POP) which is about abstraction and simplicity. POP helps to solve the [bloat that is sometimes caused by Object Oriented Programming(OOP)](http://blogs.perl.org/users/sid_burn/2014/03/inheritance-is-bad-code-reuse-part-1.html). If you ever find yourself having to inherit from multiple classes, you probably should consider using protocols instead.

Here's some code to serve as a brief introduction to POP

We first introduce our protocols.

```
protocol Bird {
  var name: String { get }
  var canFly: Bool { get }
}
 
protocol Flyable {
  var airspeedVelocity: Double { get }
}
```

Next, we introduce the structs that confrom to the protocols above.

```
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

To understand more about POP, I highly recommend watching this [2015 WWDC talk](https://www.youtube.com/watch?v=g2LwFZatfTI).

## Automatic Reference Counter

A few keywords special to Swift are `strong`, `weak` and `unowned`, which have to do with Swift's way of memory management, [Automatic Reference Counting (ARC)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AutomaticReferenceCounting.html). Essentially, when an instance of class is created, a chunk of memory is allocated to it until it is no longer strongly referenced by anything. Reference are strong by default. Thus, if we have an Object A (a `UIViewController`) that creates Object B (a `UIAlertController`), B would be strongly referenced by A. However, B might also need access to a variable in A, resulting in A being strongly referenced by B, creating a reference cycle. Reference cycles are bad, because they cause memory leaks. Even though A and B are no longer needed eventually, A and B will still sit in memory since they are both strongly referenced by each other. This is why we need the `strong`, `weak` and `unowned` keywords, to resolve reference cycles. 

[Here is an article](https://krakendev.io/blog/weak-and-unowned-references-in-swift) with greater in-depth explanation and examples


## CocoaPods

If you are working on a Swift project for awhile and you start to think: Hmm... Someone has probably done something like this before / This is a common problem and there should be a library for this. Check out [CocoaPods](https://guides.cocoapods.org/using/getting-started.html), THE package/library manager for Swift.