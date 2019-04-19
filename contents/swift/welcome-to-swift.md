<frontmatter>
  title: Swift
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Swift

**Authors: [Ch'ng Ming Shin](https://github.com/ablyx/cs3281-website/blob/mingshin-week6-progress/students/AY1617S2/ch'ngMingShin/Ch'ngMingShin-Resume.md), [Jiang Chunhui](https://github.com/Adoby7), [Yong Zhi Yuan](https://github.com/Zhiyuan-Amos)**

Reviewers: [Aaron Chong](https://github.com/acjh), [Bryan Lew](https://github.com/blewjy), [Dickson Tan](https://github.com/Neurrone), [Rachael Sim](https://github.com/rachx), [Rahul Rajesh](https://github.com/rrtheonlyone), [Sam Yong](https://github.com/mauris), [Tan Wang Leng](https://github.com/yamgent), [Vivek Lakshmanan](https://github.com/vivekscl), [Wang Junming](https://github.com/junming403), [Xiao Pu](https://github.com/xpdavid)

## Swift Overview

**Swift is the main programming language used for iOS programming.** Introduced in 2014 by Apple, Swift has more concise and more expressive syntax compared to its predecessor language [Objective-C](). Unlike most other software by Apple, Swift is [open source](https://github.com/apple/swift).

**One main attraction to learn Swift's is that iOS developers are well paid** ([example](https://www.indeed.com/salaries/iOS-Developer-Salaries)).

Swift syntax is not too different from other mainstream languages such as Python, Java or C++, which means switching to Swift is not difficult. On top of that, Swift also supports [_playgrounds_](), a feature that allows programmers to experiment with Swift code and see the results immediately, without the overhead of building and running an app.

## Noteworthy Swift Features

Here are some noteworthy Swift features for you to get a feel of Swift.

### Type Inference

Swift supports type inference whereby the compiler automatically deduces the type of a variable during compilation by examining the values assigned to it. 

```swift
var str1: String = "foo"
var str2 = "foo" // compiler infers that str2 is of type String
```

Unlike **Python** or **JavaScript** which are dynamically typed, variables in Swift are statically typed. Statically-typed languages have will have the code checked at compile-time instead of run-time, which eliminates many (often) trivial bugs early, which in turn makes debugging the program easier.

```swift
var str2 = "foo" // compiler infers that str2 is of type String
str2 = 5 // compilation error
```

### Optionals

Swift allows the use of `Optionals`, so that you can choose to either return nil or a data value, instead of returning a special value to indicate the absence of a value.

```swift
func yearAlbumReleased(name: String) -> Int {
    switch name {
    case "Taylor Swift": return 2006
    case "Fearless": return 2008
    case "Speak Now": return 2010
    case "Red": return 2012
    case "1989": return 2014
    default:
        return -1 // Special value, but is not meaningful to other developers
    }
}
```
Without Optionals, you might consider using -1 to indicate that there was no such album. However, if someone else uses this function, he may not know that -1 means "no such album", and it would be better if we could return nil?

```swift
func yearAlbumReleased(name: String) -> Int? { // "?" indicates the return value type is an optional.
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

#### Optional Binding

Then, you can unwrap the Optional safely using an `if-let` statement to distinguish whether it is `nil` or not, and handle them separately:

```swift
func timeTravel(album: String) {
    let year = yearAlbumReleased(album)
    if let past = year { 
        // year contains a non-nil value
        // past is of type Int (not Int?) with the value stored in year
    } else { 
        // year contains a nil value
    }
}
```


If you would like to read up more about optionals, take a look at these articles:
* [Swift optionals explained simply](https://hackernoon.com/swift-optionals-explained-simply-e109a4297298)
* [What is an optional value in Swift?](http://stackoverflow.com/questions/24003642/what-is-an-optional-value-in-swift)

#### Guard Statements

Notice that the [happy path](http://xunitpatterns.com/happy%20path.html) in the code above is indented:

```swift
func timeTravel(album: String) {
    let year = yearAlbumReleased(album)
    if let past = year {
        // happy path is indented
        // past contains the non-nil value of year; proceed to do something with past
    } else { 
        // failure case
    }

    // past is no longer defined; unable to use past here
}
```

With the guard statement, the happy path is not indented:

```swift
func timeTravel(album: String) {
    let year = yearAlbumReleased(album)
    guard let past = year else {
        // failure case
        return
    }

    // happy path is not indented
    // past contains the non-nil value of year; proceed to do something with past
    // past remains defined till the function exits
}
```

Let's understand how the code above works:
1. The code within a `guard` block is only executed if `year` contains a nil value.
1. As the `guard` statement is used to transfer program control out of a scope, you must call one of the following functions within the `guard` block: `return`, `break`, `continue`, `throw`. As such, the `guard` statement is meant to enforce the pre-conditions of a method and to perform early return.

Here are some of the benefits of using `guard` statement over `if-let` statement:
1. Unlike the `if-let` statement, using the `guard` statement causes `past` to remain defined and can be used till the function exits.
1. While using `if-let` statements can lead to deeply nested `if-let` statements (i.e. pyramid of doom), `guard` statements allow us to have the happy path to be not indented, thereby increasing code readability.

### Defer Statements

The `defer` key word in Swift provides an easy and safe way to execute some code before leaving current scope. It is helpful when you need to do post-operations in a function which has many points of return.

The following code is an example of using a file:
```swift
let fileDescriptor = open(url.path, O_EVTONLY)
if fileDescriptor == -1 {
	close(fileDescriptor)
	return "Failed"
}

// Use file descriptor

close(fileDescriptor)
return "Success"
```

As you can see from above, we have to close the `fileDescriptor` for every case we consider. This can be problematic when the number of cases increases. Instead, we can use the `defer` statement:

```swift
let fileDescriptor = open(url.path, O_EVTONLY)

defer {
    close(fileDescriptor)
}

if fileDescriptor == -1 {
	return "Failed"
}
// Use file descriptor
return "Success"
```

Using `defer` statement, the file will be closed no matter which branch the program returns. It also has the added advantage of preventing the developer from forgetting to close the file in some cases.

This [document](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_defer-statement) explains more about `defer` statement in Swift.

### Data Types

#### Structs

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

In the example above, when you assign a reference type variable to another (i.e. `x` and `y`) they both refer to the same memory space. Later when you modify one of the variables, the other one will refer to the new value too. This may not be the desired behavior in some cases. In those cases, value-type variables (e.g., `a` and `b`) can be used to avoid implicit data sharing.

You can think of Structs as a way to create instances that have their own unique copies of data, which can help to make things a lot easier.

If you wish to find out more, here is an [article](https://medium.com/capital-one-developers/reference-and-value-types-in-swift-de792db330b2) that explains the difference between the 2 types, as well as the benefits of value types and when to use them.

#### Enums

An enum is a data type that represents of a set of values. For example, we can use `String` to represent the possible types of a barcode. However, this allows us to assign invalid values to it:

```swift
var barcode = "qzCode" // supposed to be "qrCode", but we accidentally assigned an invalid value
```

As such, we create an enum to restrict the values that we can assign to a barcode.

```swift
enum Barcode {
    case upc
    case qrCode
}

var barcode = Barcode.qrCode 
barcode = Barcode.qzCode // compilation error
```

Swift's enums can have associated values. This enables you to store additional custom information along with each case value, and permits this information to vary each time you use that case in your code. For example, we can have an enum `Barcode` with case values `upc` and `qrCode`. We want to be able to distinguish within each value as each `upc` and `qrCode` can take on different values:

```swift
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
    
    func printCode() {
        switch self {
        case let .upc(numberSystem, manufacturer, product, check):
            print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
        case let .qrCode(productCode):
            print("QR code: \(productCode).")
        }
    }
}

let barcode1 = Barcode.qrCode("foo")
let barcode2 = Barcode.qrCode("bar")
barcode1.printCode() // prints "QR code: foo."
barcode2.printCode() // prints "QR code: bar."
```

Also, enums with associated values is not supported in languages such as [Java](https://stackoverflow.com/questions/30044334/how-can-i-create-a-java-enum-with-associated-values-like-swift-enum), and using a workaround to implement enums with associated values results in code verbosity. Take a look at [Swift's documentation on Enums](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html) for more information about enums.

### Protocol Oriented Programming

The heart of Swift is Protocol Oriented Programming (POP). POP helps to solve the [bloat that is sometimes caused by Object Oriented Programming (OOP)](http://blogs.perl.org/users/sid_burn/2014/03/inheritance-is-bad-code-reuse-part-1.html) by using composition instead of inheritance for defining new classes based on existing classes.

Here's some code to serve as a brief introduction to POP:

First, we first define our protocols.

```swift
protocol Bird {
  var canFly: Bool { get }
}
 
protocol Flyable {
  var airspeedVelocity: Double { get }
}
```

Next, we define the structs that conform to the protocols above.

```swift
// Penguins can't fly ):
struct Penguin: Bird {
    let canFly = false
}

struct Eagle: Bird, Flyable {
    let canFly = true
    let airspeedVelocity = 160.0
}
```

And if you haven't noticed, protocols are extremely similar to interfaces in Java. 

To understand more about POP, watching this [WWDC 2015 talk](https://www.youtube.com/watch?v=g2LwFZatfTI) is highly recommended.

### Extensions

Extensions allow us to add new functionalities to an existing class, structure, enumeration, or protocol type. Suppose we have an `Eagle` struct:

```swift
struct Eagle {
    // some functionalities here
}
```

As development progresses, you realize that you now want `Eagle` to conform to `Bird` and `Flyable` protocols. Instead of editing the code in `Eagle` struct directly, we can use extensions to implement each protocol separately. Do take note that you cannot add stored properties in extensions. As such, `canFly` and `airspeedVelocity` have to be computed properties (for more information, see [here](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Properties.html)):

```swift
struct Eagle {
    // we can leave the existing code here untouched
}

extension Eagle: Bird {
    var canFly: Bool {
        return true
    }
}

extension Eagle: Flyable {
    var airspeedVelocity: Double {
        return 160.0
    }
}
```

Extensions also allow us to define instance methods and type methods for types which you do not have access to the original source code. For example: 

```swift
extension String { // String belongs to Swift Standard Library which we have no access to
    // This method is copied from: 
    // https://github.com/SwifterSwift/SwifterSwift/blob/master/Sources/Extensions/SwiftStdlib/StringExtensions.swift
    func isAlphabetic() -> Bool {
        let hasLetters = rangeOfCharacter(from: .letters, options: .numeric, range: nil) != nil
        let hasNumbers = rangeOfCharacter(from: .decimalDigits, options: .literal, range: nil) != nil
        return hasLetters && !hasNumbers
    }
}

var foo: String = "a1"
print(foo.isAlphabetic()) // prints "false"
```

To find out more about extensions, take a look at [Swift's documentation on Extensions](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Extensions.html)

### Automatic Reference Counting

A few keywords unique to Swift are `strong`, `weak` and `unowned`, which have to do with Swift's way of memory management, [Automatic Reference Counting (ARC)](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/AutomaticReferenceCounting.html). 

Essentially, when an instance of a class is created, a chunk of memory is allocated to it until it is no longer strongly referenced by anything. References are strong by default. Thus, if we have an Object A (a `UIViewController`) that creates an Object B (a `UIAlertController`), B would be strongly referenced by A. However, B might also need access to a variable in A, such that A may be strongly referenced by B, resulting in a reference cycle.

Reference cycles are bad, because they cause memory leaks. Even though A and B are no longer needed eventually, A and B will still sit in memory since they are both strongly referenced by each other. This is why we need the `strong`, `weak` and `unowned` keywords, to resolve reference cycles. 

Here is an [article](https://krakendev.io/blog/weak-and-unowned-references-in-swift) with greater in-depth explanation and examples.

### CocoaPods

[CocoaPods](https://guides.cocoapods.org/using/getting-started.html) is a dependency manager for Swift and Objective-C Cocoa projects which has over 58 thousand libraries and is used in over 3 million apps. Instead of reinventing the wheel, you can check this out to obtain code that helps resolve common issues. If you have done something new with Swift, you can also make your code into a library with CocoaPods for others to use!

## How to Get Started

A Macbook is required for Swift development, but an iPhone or iPad is not. The Swift IDE `X-Code` has built-in simulators for all mobile devices.

If you have not learnt any other programming languages before, this [Game App](https://www.apple.com/swift/playgrounds/) could be a good choice to learn swift as well as programming.

If you are familiar with some programming languages, reading the [Language Guide in the official documentation](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/) is recommended, since it explains everything quite clearly, albeit being quite verbose.

If you are really pressed for time, here are a couple of cheatsheets with code examples:

- [Swift Cheat Sheet](https://mhm5000.gitbooks.io/swift-cheat-sheet/content/index.html) (web-friendly)
- [Swift 3.0 Cheat Sheet and Quick Reference](https://koenig-media.raywenderlich.com/uploads/2014/06/RW-Swift-Cheatsheet-0_7.pdf) (print-friendly)

</div>
