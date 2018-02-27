# Introduction to Kotlin

Author: [Shradheya Thakre](https://github.com/tshradheya)

**Table of Contents**

* [Overview](#overview)
* [Why Kotlin?](#why-kotlin)
    * [Concise](#concise)
    * [Safe](#safe)
    * [Interoperable](#interoperable)
    * [Tool Friendly](#tool-friendly)
* [Kotlin for Android Apps](#kotlin-for-android-apps)
    * [Advantages of Shifting to Kotlin](#advantage-kotlin)
    * [Drawbacks of Shifting to Kotlin](#drawbacks-kotlin)






# Overview

Kotlin is a general purpose, open source, statically typed “pragmatic” programming language for the JVM and Android that combines object-oriented and functional programming features.

# Why Kotlin?

### Concise

Highly reduces the amount of boilerplate code.
- A POJO with getters, setters, equals(), hashCode(), toString() and copy() can be created in a single line
``` kotlin
data class Customer(val name: String, val email: String, val company: String)
```
- Assigning values based on ranges is much simpler
``` kotlin
val quartile: Int
quartile = when (playPercentage) {
    in 0..24 -> 0
    in 25..49 -> 1
    ...
}
```


### Safe
Kotlin protects you from mistakenly operating on nullable types
- Get compilation error when you mistakenly try operating on nullable types
```
val name: String? = null    // Nullable type
println(name.length())      // Compilation error
```

### Interoperable

- Target either the JVM or JavaScript. Write code in Kotlin and decide where you want to deploy
```
import kotlin.browser.window
fun onLoad() {
    window.document.body!!.innerHTML += "<br/>Hello, Kotlin!"
}
```
- You can literally continue work on your old Java projects using Kotlin. All your favorite Java frameworks are still available, and whatever framework you’ll write in Kotlin can be easily adopted by other Java developers

### Tool Friendly

Hyperbolically, a programming language is only as good as its tool support. This is why advantage with using Kotlin is that IntelliJ provides built-in language support. It also contains the aforementioned Java-to-Kotlin converter and code generators for Java and JavaScript from Kotlin code

# Kotlin for Android Apps

While Java is one of the world's most widely used programming languages and is pretty much the official language of Android development, there are many reasons why Java might not always be the best option for your Android projects.

The biggest issue is that Java isn’t a modern language, and although Java 8 was a huge step forward for the platform, introducing lots of features that developers had been waiting for (including lambda functions), at the time of writing Android only supports a subset of Java 8 features.

### Advantages of Shifting to Kotlin

- Interchangeability With Java
- Easy Learning Curve
- Combine the Best of Functional and Procedural Programming
- First-Class Android Studio Support
- More Concise Code

### Drawbacks of Shifting to Kotlin

- Extra Runtime Size due to increase in size of `.apk`
- Initial Readability of Code for core Java developers
- Smaller Community and Less Available Help
