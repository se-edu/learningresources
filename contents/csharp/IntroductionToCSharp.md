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
public class Context
{
    //GetCounter returns a nullary (0 argument) function. The function returns an integer when executed.
    public static Func<Int> GetCounter()
    {
        //Inside the context of this function, there is an integer variable count.
        int count = 0;
        //GetCounter is returning a function.
        return () =>
        {
            //The function increments the count variable inside this context, which is initialized to 0.
            count++;
            //It then returns the current count value.
            return count;
        }
    }
}

Func<Int> counter = Context.GetCounter();
//Every time the function is called, the variable count in the captured context would increment by 1, and its new value will be returned.
counter(); //Returns 1
counter(); //Returns 2
```

If you wish to read more about closures, you may consult [this article by dixin](https://weblogs.asp.net/dixin/understanding-csharp-features-6-closure)

### Nullable type

Often when dealing with possible sites of [null pointer](https://en.wikipedia.org/wiki/Null_pointer "In computing, a null pointer or null reference has a value reserved for indicating that the pointer or reference does not refer to a valid object.") 
access, many if checks might be used. However, when dealing with nullable types, such code can be shortened
while remaining easy to read, using collaescing operators `??`, or null conditional operators `.?`. Some may find this similar to `optionals` in Swift.

More can be read about Nullables [here](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/nullable-types/)

A code example without using Nullable features.
```csharp
public Car ManufactureCar()
{
    return (hasError) ? null : new Car(param1, param2);
}

Car car = ManufactureCar();
Double fuelLeft = 0;
//null check
if (car != null)
{
    car.Drive();
    car.GetFuel();
}

DoSomethingTo(fuelLeft);
```

A code example using Nullable features. The length of the code is approximately halved. For programmers familiar to Nullable operators,
the code should be easy to read. Nullable operators can also reduce the need for indentation and the occurrences of nested indentations.

```csharp
public Car ManufactureCar() {
    return (hasError) ? null : new Car(param1, param2);
}

Car car = ManufactureCar();
//Call Drive method of car if not null
car?.Drive();

//Get amount of fuel left
Double fuelLeft = car?.GetFuel() ?? 0;

DoSomethingTo(fuelLeft);

//Explanation:
//If car is not null, one can expect the statement to evaluate to car.GetFuel().
//If car is null, car?.GetFuel() evaluates to null
//?? operator sees null, so the entire expression defaults to 0.
```

### Default Interface Implementations

Sometimes classes have a common interface but do not have a common ancestor and require the same method implementation.
A default implementation can be included in the interface to promote code reuse. Similarly, a certain class may not require certain methods listed in the interface.
An example is a stub class that is required to implement an interface. A default method can be created with the `NotImplementedException`.

### Async/Await

Often in applications, some kind of heavy tasks need to be performed. These tasks include fetching data from a server or some data storage, I/O operations that read/write to disk,
and computationally expensive tasks such as computing the shortest path from 1 point on a map to a destination of choice. If these tasks are run synchronously, they can cause the
program to freeze up, leaving the UI (application User Interface) unresponsive, and this is undesired as consumers like us do not enjoy the lack of control over the program. In
C#, this is done using the keywords `async` and `await`. More can be read up at [Microsoft's documentation](https://docs.microsoft.com/en-us/dotnet/csharp/async)

### Other features

The list of features in C# can be quite long. This article only shows you selected features for their usefulness and ability to represent code more concisely. If you wish to explore
other features, you may consult this [article](https://stackify.com/csharp-8-features/) and others online.

## Why Learn C#

Below are some common reasons why one should learn C#.

### Cross platform support

C# is built on the .NET framework and has [cross platform](https://en.wikipedia.org/wiki/Cross-platform_software "In computing, cross-platform software (also multi-platform software or platform-independent software) is computer software that is implemented on multiple computing platforms.")
support extended by the [Mono project](https://www.mono-project.com/), and similarly the complete runtime implementation from open source [CoreCL](https://github.com/dotnet/coreclr).
As Mono supports many platforms such as Windows, MacOS, Linux and even PlayStation 4, users can build for many platforms.
C# is also used by the Unity Game Engine, which has high cross-platform support for game developers.

### Popular Language

>Oct 13, 2017
>Being powerful, flexible, and well-supported enabled C# has quickly become one of the most popular programming languages available.
>Today, it is the 4th most popular programming language, with approximately 31% of all developers using it regularly. It is also the 3rd largest community on StackOverflow (which was built using C#) with more than 1.1 million topics.

>[_Why Is C# Among The Most Popular Programming Languages in The World?_ - Armina Mkhitaryan](https://medium.com/sololearn/why-is-c-among-the-most-popular-programming-languages-in-the-world-ccf26824ffcb)

>Since it's such a robust and well-rounded language, it's no surprise that C# is utilized by thousands of companies. There are 5,000 (Mar 17, 2018) [C# jobs advertised in the US alone](https://gooroo.io/analytics/skill/C-Sharp#.WqipapPwYWo)
>(and 10,000 globally), with an [average base pay of nearly $80,000](https://www.glassdoor.com/Salaries/c-net-developer-salary-SRCH_KO0,15.htm).

>[_It pays to learn to code with C# and here's why_ - Team Commerce](https://mashable.com/2018/03/17/coding-course-class-bootcamp/#om2xRzXFHGqJ)

### Game Development

The usage of C# in game development is fairly common due to the Unity game engine. Unity has great cross-platform compatibility for desktop, web, mobile and console, and has extensive support for 2D/3D games, VR/AR games
and games that require networking.

## How to Get Started

Getting started with C# is not difficult. You can download Visual Studio and follow the setup instructions for C# programming [here](https://www.guru99.com/download-install-visual-studio.html).
If you are new to C# but have some familiarity with Java, you may follow the tutorial at [Sololearn](https://www.sololearn.com/Play/CSharp). It is a rather comprehensive tutorial that teaches
fundamental syntax and concepts in C#. If you feel that certain parts of the tutorial are too simple, you can also skip them.

If you are entirely new to programming, you may find more hands on practice with simpler steps at [CSharp net](https://csharp.net-tutorials.com/getting-started/introduction/). This tutorial tends
to be more rigorous and goes through in great detail the steps, starting from installing a development environment, to writing basic C# programs and finally topics commonly used in real applications
, such as file handling and debugging. If you wish to skip certain parts of the tutorial, the structured contents are displayed on the right side of the website.

If you want to have a go at maintaining and enhancing a small project, you may find this [fictitious airline reservation system](http://1000projects.org/airline-reservation-system-a-net-project-with-code.html) project
to be a good starting point. It covers commonly used components of a software, such as UI (Application User Interface), data storage and handling, and the logic behind the program (such as buying a ticket).
More similar projects can be found at [1000projects.org](http://1000projects.org/c-projects.html).

If you feel that you have a grasp of C# fundamentals but find it difficult to write programs of bigger scale, such as an Address Book application, you may consult
[CSharp corner](https://www.c-sharpcorner.com/UploadFile/bd5be5/design-patterns-in-net/) for a list of design patterns that you may employ to better organise and plan your
program structure. Sometimes, you may find that you have problems collaborating on a C# project. This may be due to some common misconceptions and mistakes you are commiting without
realisation. You may reduce these problems by reading about [some common mistakes in C# programming](https://www.upwork.com/hiring/development/common-mistakes-in-c-sharp-programming/).

If you want to develop a desktop application for Windows, you may consult [Microsoft's documentation](https://docs.microsoft.com/en-us/visualstudio/get-started/csharp/tutorial-wpf?view=vs-2019)
on creating an application with the `Windows Presentation Foundation`, a framework that is commonly used for creating UI for Windows Applications that has many useful features for building your UI.
