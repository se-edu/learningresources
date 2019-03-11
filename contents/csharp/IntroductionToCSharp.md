<frontmatter>
  title: Introduction to C#
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Introduction to C#

Author(s:) [Yu Pei, Henry](https://github.com/YuPeiHenry)

## What is C#

C# is an Object-Oriented Programming Language on the .NET framework, and uses Garbage Collection for memory management.
C# is considred a general-purpose, multi-paradigm programming language.

## Why Learn C#

Below are some common reasons why one should learn C#.

### Cross platform support

C# is built on the .NET framework and has cross platform support extended by the [Mono project](https://www.mono-project.com/), and similarly the complete runtime implementation from open source [CoreCL](https://github.com/dotnet/coreclr).
As Mono supports many platforms such as Windows, MacOS, Linux and even PlayStation 4, users can build for many platforms.
C# is also used by the Unity Game Engine, which has high cross-platform support for game developers.

### Popular Language

From [Armina Mkhitaryan on Medium](https://medium.com/sololearn/why-is-c-among-the-most-popular-programming-languages-in-the-world-ccf26824ffcb)

Being powerful, flexible, and well-supported enabled C# has quickly become one of the most popular programming languages available.
Today, it is the 4th most popular programming language, with approximately 31% of all developers using it regularly. It is also the 3rd largest community on StackOverflow (which was built using C#) with more than 1.1 million topics.

### Job demand and opportunities

From [Team Commerce on Mashable](https://mashable.com/2018/03/17/coding-course-class-bootcamp/#om2xRzXFHGqJ)

Since it's such a robust and well-rounded language, it's no surprise that C# is utilized by thousands of companies. There are [5,000 C# jobs advertised in the US alone](https://gooroo.io/analytics/skill/C-Sharp#.WqipapPwYWo)
(and 10,000 globally), with an [average base pay of nearly $80,000](https://www.glassdoor.com/Salaries/c-net-developer-salary-SRCH_KO0,15.htm).

## Glimpse of C# Syntax

C# is a relatively high level language and is not difficult to pick up (a little more difficult than Python and a little easier than Java.)

Below is a sample code snippet of what a simple program in C# might look like:

```
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

[Click here to get started](#how-to-get-started)

## Some Noteworthy Features of C#

Most programming languages/frameworks have their own unique quirks, which can be utilised for better code quality.

### Object / Array / Collection Initializers

In C#, you can construct an Object, Array or Collection conveniently as shown. This can be useful when writing tests, as test data will be easier to read,
as compared to calling the actual constructor.

```
public class Foo {
    Bar MyBar;
}

Bar bar = new Bar();
Foo foo = new Foo() {
    MyBar = bar;
};
```

### Closures

Sometimes, it may be useful to defer execution or capture a local context for later execution. Context capturing is reflected below:

```
//Capturing local context
public class Context {
    public static Func<Int> GetCounter() {
        int count = 0;
        return () => {
            count++;
            return count();
        }
    }
}

Func<Int> counter = Context.GetCounter();
counter();
```

### Nullable type

Often when dealing with possible sites of null pointer access, many if checks might be used. However, when dealing with nullable types, such code can be shortened
while remaining easy to read, using collaescing operators `??`, or null conditional operators `.?`. Some may find this similar to `optionals` in Swift.

More can be read about Nullables [here](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/nullable-types/)

```
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

### Testing facilities

When it comes to testing, C# does not lack in variety. 3 common unit testing frameworks used are MSTest, NUnit, XUnit.

* MSTest
    * DataTest Methods and DataRows
    * Parrallelized tests
    * Parameterized data and attributes

Resources: [Overview of some features](https://dev.to/franndotexe/mstest-v2---new-old-kid-on-the-block), [Setting up MSTest](https://www.meziantou.net/2018/02/05/mstest-v2-data-tests)
    
* NUnit
    * DataTest and DataRows
    * Stubbing directly on interfaces
    * Parameterization and attributes

Resources: [Introduction to Unit Testing](https://www.typemock.com/unit-test-patterns-for-net-part-i/)


## How to Get Started

* [Learn C#](https://www.sololearn.com/Course/CSharp/?ref=medcsharp)
* [Common pitfalls](https://www.upwork.com/hiring/development/common-mistakes-in-c-sharp-programming/)
* [Design Patterns in C#](https://www.c-sharpcorner.com/UploadFile/bd5be5/design-patterns-in-net/)
* [C# closures in greater depth](https://weblogs.asp.net/dixin/understanding-csharp-features-6-closure)
