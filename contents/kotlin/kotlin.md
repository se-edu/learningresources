<frontmatter>
  title: Introduction to Kotlin
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Introduction to Kotlin

Author: [Shradheya Thakre](https://github.com/tshradheya)

**Chapter contents**

* [Overview](#overview)
* [Why Kotlin?](#why-kotlin)
    * [Concise](#concise)
    * [Safe](#safe)
    * [Interoperable](#interoperable)
    * [Tool Friendly](#tool-friendly)
* [Kotlin for Android Apps](#kotlin-for-android-apps)
    * [Advantages of Shifting to Kotlin](#advantages-of-shifting-to-kotlin)
    * [Drawbacks of Shifting to Kotlin](#drawbacks-of-shifting-to-kotlin)
* [Resources to learn Kotlin](#resources-to-learn-kotlin)

# Overview

Kotlin is a general purpose, open source, statically typed programming language for the JVM and Android that combines object-oriented and functional programming features.

# Why Kotlin?

### Concise

Highly reduces the amount of boilerplate code.
- A Java object with getters, setters, equals(), hashCode(), toString() and copy() can be created in a single line in Kotlin
``` kotlin
data class Customer(val name: String, val email: String, val company: String)
```
- Assigning values based on ranges is much simpler than Java. Below are extracts of code with similar logic in Java and Kotlin:

**Kotlin**
``` kotlin
val quartile: Int
quartile = when (playPercentage) {
    in 0..24 -> 0
    in 25..49 -> 1
    ...
}
```

**Java**
``` java
int quartile;
if(playPercentage >= 0 && playPercentage <= 24) {
    quartile = 0;
} else if(playPercentage >= 25 && playPercentage <= 49) {
    quartile = 1;
}
...
```

### Safe

Kotlin protects you from mistakenly operating on [nullable](https://kotlinlang.org/docs/reference/null-safety.html) types
- Get compilation error when you mistakenly try operating on nullable types
``` kotlin
val name: String? = null    // Nullable type
println(name.length())      // Compilation error
```

### Interoperable

- Target either the JVM or JavaScript. Write code in Kotlin and decide where you want to deploy
``` kotlin
import kotlin.browser.window
fun onLoad() {
    window.document.body!!.innerHTML += "<br/>Hello, Kotlin!"
}
```
- You can literally continue work on your old Java projects using Kotlin. All your favorite Java frameworks are still available, and whatever framework you’ll write in Kotlin can be easily adopted by other Java developers

### Tool Friendly

Hyperbolically, a programming language is only as good as what its tools can provide. This is why the advantage of using Kotlin is the built-in language support from IntelliJ. Any Java IDE for e.g. IntelliJ, Eclipse and Android Studio, can be used to write and compile Kotlin code. It also contains the aforementioned Java-to-Kotlin converter and code generators for Java and JavaScript from Kotlin code.

# Kotlin for Android Apps

While Java is one of the world's most widely used programming languages and is pretty much the official language of Android development, there are many reasons why Java might not always be the best option for your Android projects.

The biggest issue is that Java isn’t a modern language, and although Java 8 was a huge step forward for the platform, introducing lots of features that developers had been waiting for (including lambda functions), at the time of writing Android only supports a subset of Java 8 features.

### Advantages of Shifting to Kotlin

- Interchangeability With Java
- Easy learning curve
- Combine the best of functional and procedural programming
- First-class Android Studio support
- More concise code

### Drawbacks of Shifting to Kotlin

- Extra runtime size due to increase in size of `.apk`
- Initial readability of code may be hindered for core Java developers
- Smaller community support

# Resources to learn Kotlin

- [Official Resources by Android](https://developer.android.com/kotlin/resources.html)
- [Kotlin Documentation](https://kotlinlang.org/docs/reference/)
- [Cheatsheet for shifiting from Java to Kotlin](https://github.com/MindorksOpenSource/from-java-to-kotlin)

</div>
