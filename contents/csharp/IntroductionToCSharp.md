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

<box id="article-toc">

* [Introduction to C#‎](#introduction-to-c#)
    * [What is C#‎](#what-is-c#)
    * [C# Syntax Features‎](#c#-syntax-features)
        * [Object/Array/Collection Initializers‎](#object/array/collection-initializers)
        * [Closures‎](#closures)
        * [Nullable type‎](#nullable-type)
        * [Other features](#other-features)
    * [Why Learn C#‎](#why-learn-c#)
    * [How to Get Started‎](#how-to-get-started)
</box>

## What is C#

C# is a _general purpose_, _multi-paradigm_, _garbage collected_, _cross-platform_ language by Microsoft, and part of the _.NET platform_. Some claim C# is Microsoft's answer to Java due to the fact that the two languages have a lot of similarities.

Given below are brief explanations of the key characteristics of C# mentioned above.
* **General purpose**: 
  <blockquote>In computer software, a general-purpose programming language is a programming language designed to be used for writing software in the widest variety of application domains (a general-purpose language). A general-purpose programming language has this status because it does not include language constructs designed to be used within a specific application domain.<br>--(source: [Wikipedia](https://en.wikipedia.org/wiki/General-purpose_programming_language))</blockquote>
* **Multi-paradigm**: [_Programing Paradigms_](https://en.wikipedia.org/wiki/Programming_paradigm) are used to describe Programming Languages based on their features. Some commonly referred paradigms are [Object-Oriented Programming](https://en.wikipedia.org/wiki/Object-oriented_programming) (which primarily organizes code into objects that contain a state) and [Functional Programming](https://en.wikipedia.org/wiki/Functional_programming) (where code represents a sequence of stateless functions.) C# supports both Object-Oriented and Functional Programming, and many others that can be found [here](https://en.wikipedia.org/wiki/C_Sharp_(programming_language)).

* **Garbage Collected**: The intialization, storage and handling of variables require memory. *Garbage Collection* is a form of automatic memory management, where memory that is no longer referenced by the program will be deallocated. You may read more about [Garbage Collection](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)).

* **Cross-Platform**: [Cross-Platform](https://en.wikipedia.org/wiki/Cross-platform_software) software is software that can be run across multiple platforms, which may require recompilation depending on the software. Common platforms include Windows, MacOS and Linux, and for mobile platforms Android and iOS. The benefits of writing Cross-Platform software is that developers will only need to primarily maintain 1 code base and be able to deploy to multiple platforms.


Below is an example of of a simple C# program:

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

Developers that work with C# commonly use Visual Studio as their IDE and also as a build tool for compilation into common application/library formats, such as `.exe` or `.dll`. Using Visual Studio for C# development offers great convenience as it has an integrated testing framework, [MSTest](https://en.wikipedia.org/wiki/Visual_Studio_Unit_Testing_Framework).

## C# Syntax Features

This section covers some noteworthy features of C# syntax %%(some of them are found in other languages such as Java and Swift)%%.

### Object/Array/Collection Initializers

In C#, you can construct an [Object](https://en.wikipedia.org/wiki/Object_(computer_science) "In computer science, an object can be a variable, a data structure, a function, or a method, and as such, is a value in memory referenced by an identifier."), [Array](https://www.webopedia.com/TERM/A/array.html "In programming, a series of objects all of which are the same size and type. Each object in an array is called an array element. For example, you could have an array of integers or an array of characters or an array of anything that has a defined data type.") or [Collection](https://computersciencewiki.org/index.php/Collections "A collection — sometimes called a container — is simply an object that groups multiple elements into a single unit.") in a single statement as shown. This can be useful when writing tests, as test data will be better organised, as compared to calling the actual constructor.

```csharp
public BookShelf(Book[] books, param2, param3, param4)
{
    this.books = books
    // Do something with param 2, 3, 4
}

// Without use of Object Initializer
BookShelf shelf1 = new BookShelf(books, param2, param3, param4)

// With use of Object Initializer
BookShelf shelf2 = new BookShelf() {
    books = { book1, book2 };
};
```

In the above example, we want to test a `BookShelf`'s behaviors only related to the `Book[] array`. Instead of having to unnecessarily use param2, 3, 4 in construction, we can initialize a `BookShelf` only with the `Book[]` that we wanted to use.

### Closures

Sometimes, it may be useful to [defer execution](http://www.informit.com/articles/article.aspx?p=2171751 "Code that is executed only when results need to be evaluated. There are many reasons for executing code later") or capture a local context for later execution. Context capturing is reflected below:

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

The ability to capture the value `count` outside of the defined function scope that returns `count`, is called a closure. If you wish to read more about closures, you may consult [this article by dixin](https://weblogs.asp.net/dixin/understanding-csharp-features-6-closure)

### Nullable type

Normally to guard against null pointers, an `if` branch or a guard clause that checks against `null` is used. Below is a code example of conventional null pointer handling.
```csharp
public static Car ManufactureCar() {
    return (hasError) ? null : new Car(param1, param2);
}
public void AddFuelTank() {
    fuelTank =  (hasError) ? null : new FuelTank(param3);
}

Car car = Car.ManufactureCar();
Double fuelLeft = 0;
if (car != null)
{
    car.AddFuelTank();
	fuelTank = car.GetFuelTank()
	if (fuelTank != null) {
        fuelLeft = fuelTank.GetFuel();
    }
}

DoSomethingTo(fuelLeft);
```

In C#, `null` handling does not need to be done the conventional way. C# has `Nullable` features, such as collaescing operators `??` and null conditional operators `.?`. Some may find this similar to `optionals` in Swift. Applying these features appropriately not only results in shorter and more concise code in general. It makes it easier to reduce the use of indentation as well. Nullables may appear less intuitive to new users, so its value may differ between communities.

```csharp
public static Car ManufactureCar() {
    return (hasError) ? null : new Car(param1, param2);
}
public void AddFuelTank() {
    fuelTank =  (hasError) ? null : new FuelTank(param3);
}

Car car = Car.ManufactureCar();
//Call Drive method of car if not null
car?.AddFuelTank();

//Get amount of fuel left with default value 0
Double fuelLeft = car?.GetFuelTank()?.GetFuel() ?? 0;

DoSomethingTo(fuelLeft);
```

Some explanation of what `Double fuelLeft = car?.GetFuel() ?? 0` does:
* If car is not null, one can expect the statement to evaluate to `car.GetFuel()`.
* If car is null, `car?.GetFuel()` evaluates to null.
* If GetFuelTank() returns null, or car evaluates to null, car?.GetFuelTank()?.GetFuel() evaluates to null.
* `??` operator sees null, so the entire expression defaults to 0. If `??` sees a non-null value, the default value is not used.

More can be read about Nullables [here](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/nullable-types/)

### Other features

The list of features in C# can be quite long. This article only shows you selected features for their usefulness and ability to represent code more concisely. Some features that were not listed are [Async/Await](https://docs.microsoft.com/en-us/dotnet/csharp/async) and [Default Interface Implementation](https://www.infoq.com/articles/default-interface-methods-cs8). If you wish to explore other features, you may consult this [article](https://stackify.com/csharp-8-features/) and others online.

## Why Learn C#

C# is high in demand (as per [source1](https://medium.com/sololearn/why-is-c-among-the-most-popular-programming-languages-in-the-world-ccf26824ffcb), [source 2](https://mashable.com/2018/03/17/coding-course-class-bootcamp/#om2xRzXFHGqJ)). It is especially well-suited for Windows apps. It also thrives in game programming because the popualr game engine Unity has great cross-platform compatibility for desktop, web, mobile and console, and has extensive support for 2D/3D games, VR/AR games and games that require networking. C# can even be used on non-Windows environments as the .NET framework has [cross platform](https://en.wikipedia.org/wiki/Cross-platform_software "In computing, cross-platform software (also multi-platform software or platform-independent software) is computer software that is implemented on multiple computing platforms.") support via the [Mono project](https://www.mono-project.com/).


## How to Get Started

Getting started with C# is not difficult. You can download Visual Studio and follow the setup instructions for C# programming [here](https://www.guru99.com/download-install-visual-studio.html). If you are new to C# but have some familiarity with Java, you may follow the tutorial at [Sololearn](https://www.sololearn.com/Play/CSharp). It is a rather comprehensive tutorial that teaches fundamental syntax and concepts in C#. If you feel that certain parts of the tutorial are too simple, you can also skip them.

If you are entirely new to programming, you may find more hands on practice with simpler steps at [CSharp net](https://csharp.net-tutorials.com/getting-started/introduction/). This tutorial tends to be more rigorous and goes through in great detail the steps, starting from installing a development environment, to writing basic C# programs and finally topics commonly used in real applications, such as file handling and debugging. If you wish to skip certain parts of the tutorial, the structured contents are displayed on the right side of the website.

If you want to have a go at maintaining and enhancing a small project, you may find this [fictitious airline reservation system](http://1000projects.org/airline-reservation-system-a-net-project-with-code.html) project
to be a good starting point. It covers commonly used components of a software, such as UI (Application User Interface), data storage and handling, and the logic behind the program (such as buying a ticket). More similar projects can be found at [1000projects.org](http://1000projects.org/c-projects.html).

If you feel that you have a grasp of C# fundamentals but find it difficult to write programs of bigger scale, you may consult
[CSharp corner](https://www.c-sharpcorner.com/UploadFile/bd5be5/design-patterns-in-net/) for a list of design patterns that you may employ to better organise and plan your program structure. Sometimes, you may find that you have problems collaborating on a C# project. This may be due to some common misconceptions and mistakes you are commiting without realisation. You may reduce these problems by reading about [some common mistakes in C# programming](https://www.upwork.com/hiring/development/common-mistakes-in-c-sharp-programming/).

If you want to develop a desktop application for Windows, you may consult [Microsoft's documentation](https://docs.microsoft.com/en-us/visualstudio/get-started/csharp/tutorial-wpf?view=vs-2019) on creating an application with the `Windows Presentation Foundation`, a framework that is commonly used for creating UI for Windows Applications that has many useful features for building your UI.
</div>
