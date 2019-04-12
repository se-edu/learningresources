<frontmatter>
  title: Introduction to C#
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Introduction to C#

**Author(s:) [Yu Pei, Henry](https://github.com/YuPeiHenry)**

Reviewers: [Chester Sng](https://github.com/ChesterSng), [Lin Si Jie](https://github.com/sijie123)

## What is C#

C# is a _general purpose_, _multi-paradigm_, _garbage collected_, _cross-platform_ language by Microsoft, and part of the _.NET platform_. Some claim C# is Microsoft's answer to Java.

_general purpose_
>From [Wikipedia](https://en.wikipedia.org/wiki/General-purpose_programming_language)
>In computer software, a general-purpose programming language is a programming language designed to be used for writing software in the widest variety of application domains (a general-purpose language).
>A general-purpose programming language has this status because it does not include language constructs designed to be used within a specific application domain.

_multi-paradigm_
**Programing Paradigms** are used to describe Programming Languages based on their features. Some commonly referred paradigms are [Object-Oriented Programming](https://en.wikipedia.org/wiki/Object-oriented_programming)
(which primarily organizes code into objects that contain a state) and [Functional Programming](https://en.wikipedia.org/wiki/Functional_programming) (where code represents a sequence of stateless functions.)
C# supports both Object-Oriented and Functional Programming, and many others that can be found [here](https://en.wikipedia.org/wiki/C_Sharp_(programming_language)).
You may also read more about [multi-paradigm](https://en.wikipedia.org/wiki/Programming_paradigm) programming languages.

_garbage collected_
The intialization, storage and handling of variables require memory. *Garbage Collection* is a form of automatic memory management, where memory that is
no longer referenced by the program will be deallocated. You may read more about [Garbage Collection](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)).

_cross-platform_
[Cross Platform](https://en.wikipedia.org/wiki/Cross-platform_software) software is software that can be run across multiple platforms, which may require recompilation depending on the software. Common platforms include Windows, MacOS and Linux, and for mobile platforms Android and iOS.
The benefits of writing **Cross Platform** software is that developers will only need to primarily maintain 1 code base and be able to deploy to multiple platforms.

### Glimpse of C#

C# is a relatively high level language and is not difficult to pick up (a little more difficult than Python and a little easier than Java.)

Below is a sample code snippet of what a simple program in C# might look like:

```csharp
//Comments can be marked with // or /**/
//Namespaces are similar to packages, except the file does not need to be physically in the directory
using ProjectName.Utils;

namespace ProjectName.Model
{
    //Subclassing BaseClass
    class MyClass : BaseClass {
        ...
        //Method declaration
        public static void Main(String[] args) {
            //Variable definition
            string message = "Hello World!";
            Console.WriteLine(message);
        }
    }
}
```

Developers that work with C# commonly use Visual Studio to maintain their code and also as a build tool for compilation into common application/library formats, such as `.exe` or `.dll`.
Using Visual Studio for C# development offers great convenience as it has an integrated testing framework, [MSTest](https://en.wikipedia.org/wiki/Visual_Studio_Unit_Testing_Framework).

## C# Syntax Features

Most programming languages/frameworks have their own unique quirks, which can be utilised for better code quality. This section covers some **interesting and noteworthy features**
that can help make code more concise, and most are common to other languages such as Java and Swift.

### Object / Array / Collection Initializers

In C#, you can construct an [Object](https://en.wikipedia.org/wiki/Object_(computer_science) "In computer science, an object can be a variable, a data structure, a function, or a method, and as such,
is a value in memory referenced by an identifier.")
, [Array](https://www.webopedia.com/TERM/A/array.html "In programming, a series of objects all of which are the same size and type. Each object in an array is called an array element. For example,
you could have an array of integers or an array of characters or an array of anything that has a defined data type.") 
or [Collection](https://computersciencewiki.org/index.php/Collections "A collection — sometimes called a container — is simply an object that groups multiple elements into a single unit.") conveniently as shown.
This can be useful when writing tests, as test data will be easier to read, as compared to calling the actual constructor.

```csharp
BookShelf shelf = new BookShelf() {
    Books = { book1, book2 };
};
```

### Closures

Sometimes, it may be useful to [defer execution](http://www.informit.com/articles/article.aspx?p=2171751 "Code that is executed only when results need to be evaluated. There are many reasons for executing code later") 
or capture a local context for later execution. Context capturing is reflected below:

```csharp
//Capturing local context
public class Context {
    public static Func<Int> GetCounter() {
        int count = 0;
        return () => {
            count++;
            return count;
        }
    }
}

Func<Int> counter = Context.GetCounter();
counter();
```

### Nullable type

Often when dealing with possible sites of [null pointer](https://en.wikipedia.org/wiki/Null_pointer "In computing, a null pointer or null reference has a value reserved for indicating that the pointer or reference does not refer to a valid object.") 
access, many if checks might be used. However, when dealing with nullable types, such code can be shortened
while remaining easy to read, using collaescing operators `??`, or null conditional operators `.?`. Some may find this similar to `optionals` in Swift.

More can be read about Nullables [here](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/nullable-types/)

```csharp
public Car? ManufactureCar() {
    return (hasError) ? null : new Car(param1, param2);
}

Car? car = ManufactureCar();
//Call Drive method of car if not null
car?.Drive();

//Get amount of fuel left
Double fuelLeft = car?.GetFuel() ?? 0;
//Explanation:
//If car is not null, one can expect the statement to evaluate to car.GetFuel().
//If car is null, car?.GetFuel() evaluates to null
//?? operator sees null, so the entire expression defaults to 0.
```

### Default Interface Implementations

Sometimes classes have a common interface but do not have a common ancestor and require the same method implementation.
A default implementation can be included in the interface to promote code reuse. Similarly, a certain class may not require certain methods listed in the interface.
An example is a stub class that is required to implement an interface. A default method can be created with the `NotImplementedException`.

The following links below describes in greater detail of some exciting features that can make your code more pleasant to read and write, such as `async/await`, `delegates` etc:
* https://codeaddiction.net/articles/15/10-features-in-c-that-you-really-should-learn-and-use
* https://stackify.com/csharp-8-features/
* https://www.developer.com/net/csharp/slideshows/top-10-csharp-6.0-language-features.html

## Why Learn C#

Below are some common reasons why one should learn C#.

### Cross platform support

C# is built on the .NET framework and has [cross platform](https://en.wikipedia.org/wiki/Cross-platform_software "In computing, cross-platform software (also multi-platform software or platform-independent software) is computer software that is implemented on multiple computing platforms.")
support extended by the [Mono project](https://www.mono-project.com/), and similarly the complete runtime implementation from open source [CoreCL](https://github.com/dotnet/coreclr).
As Mono supports many platforms such as Windows, MacOS, Linux and even PlayStation 4, users can build for many platforms.
C# is also used by the Unity Game Engine, which has high cross-platform support for game developers.

### Popular Language

>Being powerful, flexible, and well-supported enabled C# has quickly become one of the most popular programming languages available.
>Today, it is the 4th most popular programming language, with approximately 31% of all developers using it regularly. It is also the 3rd largest community on StackOverflow (which was built using C#) with more than 1.1 million topics.

>[_Why Is C# Among The Most Popular Programming Languages in The World?_ - Armina Mkhitaryan](https://medium.com/sololearn/why-is-c-among-the-most-popular-programming-languages-in-the-world-ccf26824ffcb)

>Since it's such a robust and well-rounded language, it's no surprise that C# is utilized by thousands of companies. There are [5,000 C# jobs advertised in the US alone](https://gooroo.io/analytics/skill/C-Sharp#.WqipapPwYWo)
>(and 10,000 globally), with an [average base pay of nearly $80,000](https://www.glassdoor.com/Salaries/c-net-developer-salary-SRCH_KO0,15.htm).

>[_It pays to learn to code with C# and here's why_ - Team Commerce](https://mashable.com/2018/03/17/coding-course-class-bootcamp/#om2xRzXFHGqJ)

### Game Development

The usage of C# in game development is fairly common due to the Unity game engine. Unity has great cross-platform compatibility for desktop, web, mobile and console, and has extensive support for 2D/3D games, VR/AR games
and games that require networking.

## How to Get Started

* [Learn C#](https://www.sololearn.com/Course/CSharp/?ref=medcsharp) is a good resource to get started with C# and OOP.
* [Common pitfalls](https://www.upwork.com/hiring/development/common-mistakes-in-c-sharp-programming/) covers some common mistakes made when programming in C#.
* [Design Patterns in C#](https://www.c-sharpcorner.com/UploadFile/bd5be5/design-patterns-in-net/) introduces some common design patterns that can make planning and maintaining code easier.
* [C# closures in greater depth](https://weblogs.asp.net/dixin/understanding-csharp-features-6-closure)
