<frontmatter>
  title: Introduction to Dart
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Introduction to Dart
**Author(s): [Tan Yuanhong](https://github.com/le0tan)**<br>

## What Is Dart?
Dart (previously also known as `dartlang`) is an object-oriented, <tooltip content="a style of Object-oriented programming (OOP) in which inheritance occurs via defining classes of objects, instead of inheritance occurring via the objects alone (compare prototype-based programming)">class defined</tooltip>, garbage-collected language using a C-style syntax that <tooltip content="also known as source-to-source compilation">transcompiles</tooltip> optionally into JavaScript. It's rumored that Dart was invented out of Google engineers' frustration with JavaScript (they even built [a version of Chromium browser](http://dartdoc.takyam.com/tools/dartium/) with Dart VM so that Dart code can be run on the web without transcompiling to JavaScript). However, as most developers still stick to JavaScript and it turned out that TypeScript is a much more widely-accepted solution for statically typed JavaScript transcompilation, Dart is then, with the emergence of <tooltip content="a cross-platform mobile UI framework developed by Google">Flutter</tooltip>, re-purposed as a **client-optimized** language that's optimized for **UI creation** and **cross-platform execution**.

## Why Learn Dart?
### Flutter
**If you are to try Flutter, you have to learn Dart. :smirk:** Let's face it, Dart is basically <tooltip content="at least to most developers, Dart has never been dead inside Google as Google's most critical business - Google Ads is written entirely with Dart.">"revived"</tooltip> by the development of Flutter, and that's not a bad thing at all - there are many excellent programming languages that don't receive much attention simply because they're not used in production by huge companies or by an active community. In other words, as long as Google is still committed to maintaining Flutter and the community is still building apps using Flutter, Dart remains an actively updated programming language.
### Tooling support
Dart has out-of-the-box dependency management ([pub.dev](https://pub.dev/)), linting solution ([dartfmt](https://dart.dev/tools/dartfmt)), documentation generator ([dartdoc](https://dart.dev/tools/dartdoc)) and [official testing framework](https://pub.dev/packages/test). After installing Dart SDK, you're ready to go 99% of the time.
### Easy to learn
Like many devs who tried Dart says, you might already know Dart. If you already have *any* prior programming experience in any language (say, C, C++, Java, JavaScript, Python, Kotlin, Swift), you're pretty likely to master Dart within weeks for the following reasons:
* Dart is using a C-style syntax: it won't be a surprise if a function written in Java can be compiled and run in Dart with minor (or no) changes.
* Dart is a **strongly-typed** language with type-inference support: static typing makes IDE much more helpful and if your program compiles, it most likely works just fine.
* You may find your favorite syntax (sugar) in Dart, for example
  * Similar to JavaScript: arrow functions, named parameters (more on that later)
  * Similar to Kotlin: `get` and `set` keywords for OOP, null safety syntax
### Versatility
Dart can run on all major desktop and mobile platforms and can be transpiled to JavaScript for browser execution. It even has three execution modes (again, supporting all major platforms) for the best of all words:
* for fast development, <tooltip content="Just-in-time">JIT</tooltip> + Dart VM; 
* for native performance: <tooltip content="Ahead-of-time">AOT</tooltip> compilation to platform-specific instructions; 
* for browser support: source-to-source compiler to JavaScript
Moreover, Dart has interoperability with [C](https://dart.dev/guides/libraries/c-interop) and [JavaScript](https://dart.dev/web/js-interop) (i.e. you may execute C or JavaScript code directly from your Dart program). In Flutter, you may also invoke OS-specific methods using [platform channels](https://flutter.dev/docs/development/platform-integration/platform-channels). With regards to this point, Dart is similar to Python as a "glue language".
### Designed for Declarative UI
**Dart is a useful declarative UI language.** Dart's syntax allows you to write statically typed code in a JSON-like way, and writing asynchronous code is a breeze in Dart (more on those later).

Of course, like any other language, Dart is [not perfect](https://news.ycombinator.com/item?id=16475074). Even if Dart appears to be open-sourced, it has been pretty internally focused. And unlike Java, C++ or Python, although technically you can write most types of application using Dart, it may not be your safest choice a lot of the times (i.e. you may find yourself writing Dart in a Flutter project most of the time).

## What makes Dart appealing?

### Benefit: Asynchrony support

There are three major ways Dart supports asynchronous programming
* [`Future`](https://api.dart.dev/stable/2.7.1/dart-async/Future-class.html) object, which is very similar to [JavaScript promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
* [`Stream`](https://api.dart.dev/stable/2.7.1/dart-async/Stream-class.html) object, which can be "subscribed" to
* `async` and `await` syntax

Asynchronous programming is commonly used in UI because certain operations are not instant, most common of which is HTTP requests:

```dart
import 'dart:html';

main(List<String> args) {
  Future<String> respFuture = HttpRequest.getString('https://jsonplaceholder.typicode.com/todos/1');
  respFuture.then((result) => print(result));
  print("Hello world");
}
```

<panel header="DartPad example"><iframe style="width: 100%; height: 400px;" src="https://dartpad.dev/embed-inline.html?id=9dfa68887ae9b57f87d2eccfd061c675&split=50"></iframe></panel>

You should expect "Hello world" to be printed before the HTTP request's result because `Future` object makes sure that the progress of `main` will not be hindered by the HTTP request. Instead, *callback function* `(result) => print(result)` is called whenever the result is ready - thus **asynchronous**.

However it's possible to enforce the "line-by-line" execution order by using `await` in an `async` function:

```dart
import 'dart:html';

main(List<String> args) async {
  var result = await HttpRequest.getString('https://jsonplaceholder.typicode.com/todos/1');
  print(result);
  print("Hello world");
}
```

<panel header="DartPad example"><iframe style="width: 100%; height: 400px;" src="https://dartpad.dev/embed-inline.html?id=1aa4acd76e7a874844564913d26a72b3&split=50"></iframe></panel>

You may understand `await` as "waiting for the result, do not proceed until it's ready". `.then` and `await` are two different ways of consuming the result of a `Future`. To help you better understand the difference between `async-await` and `then`, here is an example:

<panel header="Example">

If you are to write callback functions for futures `f1` and `f2` separately, they will execute "in parallel". You should expect to see "String from the future" before "Must come second".

```dart
main() {
  var f1 = Future.delayed(Duration(seconds: 1), () => 'String from the future');
  var f2 = Future.delayed(Duration(seconds: 2), () => 'Must come second');
  f1.then((res1) => print(res1));
  f2.then((res2) => print(res2));
}
```

<panel header="DartPad example"><iframe style="width: 100%; height: 400px;" src="https://dartpad.dev/embed-inline.html?id=ebdcc9d8de89c26ceed7b1255bf40395&split=50"></iframe></panel>

However, if you only want to request the content of `f2` after `f1` is ready, you may nest callback functions like below:

```dart
main() {
  var f1 = Future.delayed(Duration(seconds: 1), () => 'String from the future');
  var f2 = Future.delayed(Duration(seconds: 2), () => 'Must come second');
  f1.then((res1) {
    f2.then((res2) {
      print(res1 + " " + res2);
    });
  });
}
```

<panel header="DartPad example"><iframe style="width: 100%; height: 400px;" src="https://dartpad.dev/embed-inline.html?id=ff4a94d26a59c373b7cd94cd8df7094a&split=50"></iframe></panel>

This time, after 2 seconds, two strings should be printed out at the same time. However, using `async-await`, there's a more elegant way to do the same thing:

```dart
main() {
  var f1 = Future.delayed(Duration(seconds: 1), () => 'String from the future');
  var f2 = Future.delayed(Duration(seconds: 2), () => 'Must come second');
  f1.then((res1) async {
    print(res1 + " " + await f2);
  });
}
```
<panel header="DartPad example"><iframe style="width: 100%; height: 400px;" src="https://dartpad.dev/embed-inline.html?id=077d288e4a705324c4aa268810904305&split=50"></iframe></panel>
</panel>

### Benefit: Extension methods
Extension methods aim to solve one problem: when using an external library, you may want to extend or even change some of the methods for your own needs. For example, instead of using `int.parse(42)`, you want to extend the `String` class so that it has a method `parseInt()` to parse a string to int. You can easily do so by extending `String` class in Dart:

```dart
extension NumberParsing on String {
  int parseInt() {
    return int.parse(this);
  }
}

main(List<String> args) {
  print('42'.parseInt());
}
```

<panel header="DartPad example"><iframe style="width: 100%; height: 400px;" src="https://dartpad.dev/embed-inline.html?id=c162704838776bb7da6568c3c1589f69&split=50"></iframe></panel>

Extending classes, especially core classes like `String`, could be quite tricky if not impossible without extension methods. However, in Dart, you can easily customize the behavior of built-in classes like so. Extension methods in Dart have even more interesting stuff like [generic support](https://dart.dev/guides/language/extension-methods#implementing-generic-extensions).

### Benefit: Named parameters
This is what makes Dart code look like JavaScript code (or JSON-like, as mentioned before). In languages like Java, the order of a function's parameters is specified by the function's definition, and if you only look at the function call, it might be difficult for you to figure out what those parameters mean. One classic example is the `put(K key, V value)` method of  `Map<K, V>` interface in Java - if you only see `m.put(1, 2)`, can you tell which one is key without prior knowledge?

That's when named parameters become handy:

When calling a function, you can specify named parameters using `paramName: value`. For example:
```dart
enableFlags(bold: true, hidden: false);
```

When defining a function, use `{param1, param2, ...}` to specify named parameters:
```dart
/// Sets the [bold] and [hidden] flags ...
void enableFlags({bool bold, bool hidden}) {...}
```

Note that if named parameters are used, the order no longer matters, you can write `enableFlags(hidden: false, bold: true);` as well. This feature is especially useful for declarative UI as the properties of a widget are, most of the time, parameters passed to the constructor of the object. Below is a concrete example in Flutter. Imagine how messy and frustrating the code would look like if the parameters are not named and we have to refer to the documentation for the exact order of them!

```dart
// Copyright 2018 The Flutter team. All rights reserved.
// Use of this source code is governed by a BSD-style license that can be
// found in the LICENSE file.

import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Welcome to Flutter',
      home: Scaffold(
        appBar: AppBar(
          title: Text('Welcome to Flutter'),
        ),
        body: Center(
          child: Text('Hello World'),
        ),
      ),
    );
  }
}
```

<panel header="DartPad example"><iframe style="width: 100%; height: 400px;" src="https://dartpad.dev/embed-flutter.html?id=719143b1a625dbdefb853c96de0ddbc2&split=50"></iframe></panel>

Note that **named** parameters are a type of [**optional** parameters](https://dart.dev/guides/language/language-tour#optional-parameters) (i.e. you can omit those parameters and they will be assigned `null` or the default value upon entry of the function). You can also make use of [`@required`](https://pub.dev/documentation/meta/latest/meta/required-constant.html) annotation to make certain named parameters compulsory.

### Benefit (advanced): Interoperability
You can call C and JavaScript code from Dart using [dart:ffi](https://api.dart.dev/dev/2.8.0-dev.7.0/dart-ffi/dart-ffi-library.html) for C or [js](https://pub.dev/packages/js) for JavaScript. These libraries translate data types from other languages to native Dart data types for mixed operation.

## How to Get Started with Dart?

Dart is well-documented and you should find solutions to most of your Dart problems on [their website](https://dart.dev/guides). You may get the Dart SDK [here](https://dart.dev/get-dart). Listed below are some useful resources for you to get started with Dart:

For beginners
- [Dart tutorials](https://dart.dev/tutorials)

For experienced programmers
- [A tour of the Dart language](https://dart.dev/guides/language/language-tour)
- [Effective Dart](https://dart.dev/guides/language/effective-dart)
- [Dart Academy](https://dart.academy/)

</div>