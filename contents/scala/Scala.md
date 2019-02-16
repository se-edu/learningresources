<frontmatter>
  title: Introduction to Scala
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Introduction to Scala

Author: [Wang Chao](https://github.com/fzdy1914)

**Table of Contents**

* [Overview](#overview)
* [Getting Started](#getting-started)
  * [Setting up using an IDE](#setting-up-using-an-ide)
    * [Installation](#installation)
    * [Creating the Project](#creating-the-project)
    * [Writing code](#writing-code)
    * [Running code](#running-code)
  * [Setting up using the command line](#setting-up-using-the-command-line)
    * [Installation](#installation-2)
    * [Creating the Project](#creating-the-project-2)
    * [Running the project](#running-the-project)
    * [Modifying the code](#modifying-the-code)
    
# Overview
**Scala** is a *multi-paradigm*, *statically typed* programming language. 
It is designed to be a mixture of *Object-Oriented Programming* and *Functional Programming*. 

Scala is **statically typed**, which means it do type checking like verifying and enforcing the constraints of types 
at compile-time.

Scala is a **pure OOP language**, which means every value in it is an object, including functions and primitives.

Scala is not a **pure Functional Programming language**, but it contains many features of it including *currying*, 
*type inference*, *immutability*, *lazy evaluation*, and *pattern matching*.

# Getting Started
There are mainly two ways people prefer to work in Scala.

1. Using an **IDE**.
2. Using the **command line**.

The following tutorials will lead you through the set up process for either way you prefer.

## Setting up using an IDE

**IntelliJ** is the most commonly-used IDE by Scala developers. 
We'll show you how to download and sett up IntelliJ with the Scala plugin.

More details can be find [here](https://docs.scala-lang.org/getting-started-intellij-track/getting-started-with-scala-in-intellij.html)

### Installation
1. Make sure you have the Java JDK 8 (version 1.8) or higher
    * Run `javac -version` on the command line to verify the version of JDK
    * If you don't have **version 1.8** or higher, install the [JDK](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
2. Next, download and install [IntelliJ Community Edition](https://www.jetbrains.com/idea/download/)
3. Then, after starting up IntelliJ, search for **"Scala"** plugin following the instructions ([How to install IntelliJ plugins](https://www.jetbrains.com/help/idea/managing-plugins.html)) 
4. Finally, download and install the Scala plugin.

### Creating the Project
1. Open up **IntelliJ** and click `File => New => Project`
2. On the left panel, select **Scala**. On the right panel, select **IDEA**.
3. Name the project `HelloWorld`
4. Select the **JDK** and **Scala SDK** for the project.

Note: If this is your **first time** creating a Scala project with IntelliJ, you'll need to install a Scala SDK. 

  * To the right of the Scala SDK field, click the `Create` button.
  * Select the **highest** version number (e.g. 2.12.8) and click `Download`. This might take a few minutes.
  * Once the SDK is created and you're back to the **"New Project"** window click `Finish`.

Note: If you want to open an existing Scala project, you can click Open when you start IntelliJ.

### Writing code
1. On the Project pane on the left, right-click **src** and select `New => Scala class`.
2. Name the class `Hello` and change the Kind to **object**.
3. Change the code in the class to the following:
```
object Hello extends App {
  println("Hello, World!")
}
```

### Running code
* Right click on `Hello` in your code and select `Run Hello`.
* You are able to see the **"Hello, World"** output in the console.

## Setting up using the command line
This part will show you how to create a Scala project from a template, which can be used as a starting point 
for your own projects. We’ll use **sbt**, the de facto build tool for Scala. sbt compiles, runs, and tests 
your projects among other related tasks. We assume you know how to use a terminal.

More details can be find [here](https://docs.scala-lang.org/getting-started-sbt-track/getting-started-with-scala-and-sbt-on-the-command-line.html)

### Installation
1. Make sure you have the Java JDK 8 (version 1.8) or higher
    * Run `javac -version` on the command line to verify the version of JDK
    * If you don't have **version 1.8** or higher, install the [JDK](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
2. Install **sbt** ([Mac](http://www.scala-sbt.org/1.x/docs/Installing-sbt-on-Mac.html), 
[Windows](http://www.scala-sbt.org/1.x/docs/Installing-sbt-on-Windows.html), [Linux](http://www.scala-sbt.org/1.x/docs/Installing-sbt-on-Linux.html))

### Creating the Project
1. `cd` to an empty folder.
2. Run the following command `sbt new scala/hello-world.g8`. It pulls the **"hello-world"** template from GitHub. 
It will also create a target folder, which you can ignore.
3. When prompted, name the application `hello-world`. This will create a project called **"hello-world"**.

### Running the project
1. `cd` into **"hello-world"**.
2. Run the command `sbt`. This will open up the sbt console.
3. Run the command `~run`. The `~` is optional and causes sbt to re-run on every file save, allowing for a fast edit/run/debug cycle.

### Modifying the code
1. Open the file `src/main/scala/Main.scala` in your favorite text editor.
2. Change **"Hello, World!"** to **"Hello, Scala!"**
3. If you haven't stopped the sbt command, you are able to see **"Hello, Scala!"** in the console.
4. You can continue to make changes as a start point of your own project.

</div>