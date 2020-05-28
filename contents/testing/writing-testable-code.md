<frontmatter>
  title: Writing Testable Code
  pageNav: 3
</frontmatter>

<div class="website-content">

# Writing Testable Code

<box id="article-toc">

* [Flaw #1: No Seams for Isolating the Class Under Test](#flaw-1-no-seams-for-isolating-the-class-under-test)
  * [Rule #1: Don't do Actual Work in Constructors](#rule-1-don-t-do-actual-work-in-constructors)
  * [Rule #2: Don't Mix Object Construction With Application Logic](#rule-2-don-t-mix-object-construction-with-application-logic)
  * [Rule #3: Don't Mix Service Objects With Value Objects](#rule-3-don-t-mix-service-objects-with-value-objects)
  * [Rule #4: Avoid Static Methods](#rule-4-avoid-static-methods)
  * [Rule #5: Favor Composition Over Inheritance](#rule-5-favor-composition-over-inheritance)
* [Flaw #2: Brittle Global State & Singleton](#flaw-2-brittle-global-state-and-amp-singleton)
  * [Rule #1: Avoid Global States](#rule-1-avoid-global-states)
  * [Rule #2: Avoid Singleton Pattern](#rule-2-avoid-singleton-pattern)
* [Flaw #3: Dig Into Collaborators](#flaw-3-dig-into-collaborators)
  * [Rule #1: Don't Look for Things. Ask for Things.](#rule-1-don-t-look-for-things-ask-for-things)
* [Flaw #4: Class Does Too Much](#flaw-4-class-does-too-much)
  * [Rule #1: Avoid Mixing of Concerns](#rule-1-avoid-mixing-of-concerns)
  * [Rule #2: Favor Polymorphism Over Conditionals](#rule-2-favor-polymorphism-over-conditionals)
* [Concluding Notes](#concluding-notes)
</box>

When the project is big enough and needs to be maintainable in the long run, it has to rely on automated tests to keep up its quality. Compared to system testing where you test the program as a whole, unit testing has its benefit for being fast (because it only instantiates a small piece of the program) and stable (because it usually mocks out the unstable dependency, e.g. network connection, database connection). Because of this, having automated unit tests becomes extremely important for Object-Oriented programs.

This article compiles multiple posts from [Google Testing Blog](https://testing.googleblog.com) on how to write more unit-testable code. It explains [four common flaws](https://testing.googleblog.com/2008/11/guide-to-writing-testable-code.html) in untestable code, and rules to follow for each of the flaws. At the end of each rule, you will see link(s) to the original Google Testing Blog posts.

## Flaw #1: No Seams for Isolating the Class Under Test

Seams is where we prevent the execution of normal code path and is how we achieve isolation of the class under test.

To create seams in your code, you need to acquire your dependency via [dependency injection](https://en.wikipedia.org/wiki/Dependency_injection). You inject real object for production and [test doubles](https://www.martinfowler.com/bliki/TestDouble.html) for testing. For example, if a class depends on some database connection to retrieve some user data, you can inject a fake database connection to always return prepared data immediately without really performing any database operation.

That is the basic idea for how seams can help in unit testing. Below are five rules you can follow to create seams in your code.

*Sources*: [Static Methods are Death to Testability](https://testing.googleblog.com/2008/12/static-methods-are-death-to-testability.html)

### Rule #1: Don't do Actual Work in Constructors

#### Warning Signs

* `new` keyword in a constructor or at field declaration
* Code does complex object graph construction inside a constructor rather than using a factory or builder
* Anything more than field assignment in constructors

Any work you do in a constructor needs to successfully navigate through in every test (not just the direct test, but also any related test which tries to instantiate your class indirectly as part of some larger object graph).

In short, the constructor's job is to assign the dependencies to fields. And that's all.

*Sources*: [Writing Testable Code](https://testing.googleblog.com/2008/08/by-miko-hevery-so-you-decided-to.html).

### Rule #2: Don't Mix Object Construction With Application Logic

#### Warning Signs

* Create objects using `new` keyword freely anywhere in your code

You should have two kinds of classes in your application.

First, **[factories](https://en.wikipedia.org/wiki/Factory_(object-oriented_programming))**, where all `new` operators reside in, are in charge of constructing objects, and nothing else.

The other, **application classes**, are devoid of `new` operators. Instead of creating object, they simply ask for them. This makes it easier to test the application logic by replacing the real classes for test doubles.

*Sources*: [Where Have all the "new" Operators Gone?](https://testing.googleblog.com/2008/09/where-have-all-new-operators-gone.html) and [Writing Testable Code](https://testing.googleblog.com/2008/08/by-miko-hevery-so-you-decided-to.html)

### Rule #3: Don't Mix Service Objects With Value Objects

Value objects are your **model** objects, like `User`, `Email`, `CreditCard`.

1. They are (or should be) very easy to construct. And they should never take a service object in its constructor (since otherwise it is not easy to construct).
2. You can freely create value objects with the "new" operator directly in line with your business logic (exception to previous rule) since they are leafs of your application graph).
3. They are **never mocked**.

Service objects, on the other hand, are your **logic** objects, like `UserAuthenticator`, `MailServer`, `CreditCardProcessor`. Compared to value objects,

1. Service objects are harder to construct. And their constructors ask for lots of other objects for collaboration.
2. Service objects are harder to construct and as a result are never constructed with a new operator in-line, (instead use factory / DI-framework) for the object graph construction.
**Note**: service objects don't take value objects in their constructors since DI-frameworks tend to be unaware about the how to create a value objects.
3. Service objects are harder to test since they are all about collaboration and as a result we are forced to use mocking, something which we want to minimize.

Clearly, they are very different in their nature and usage. Mixing the two creates a hybrid which has no advantages of value-objects and all the baggage of service-object.

*Sources*: [Writing Testable Code](https://testing.googleblog.com/2008/08/by-miko-hevery-so-you-decided-to.html).

### Rule #4: Avoid Static Methods

Static methods are procedural code. You can write your program in an OO language only using static methods, with one calling another, then it is not an OOP application but a procedural application. There is *no way* to unit test procedure application because there is no seams for you to divert the normal execution flow.

You can remove the static methods as below:

1. If the static method has arguments, chances are you can move the method as an instance method to one of the method's arguments. (As in `method(a,b)` becomes `a.method(b)`)
2. If the static method takes no arguments, either it returns a constant in which case there is nothing to test; or it accesses [global state](#rule-2-avoid-singletons), which is bad.

*Sources*: [Writing Testable Code](https://testing.googleblog.com/2008/08/by-miko-hevery-so-you-decided-to.html).

### Rule #5: Favor Composition Over Inheritance

At run-time you can not choose a different inheritance, but you can chose a different composition. This is important for tests as we want to test things in isolation.

Wrongly used inheritance clutters the focus of test because you need to mock out the your parent classes' irrelevant dependencies, too. For example,

Inheriting from AuthenticatedServlet will make your sub-class very hard to test since every test will have to mock out the authentication. But what if AuthenticatedServlet inherits from DbTransactionServlet? (that gets so much harder)

*Sources*: [Static Methods are Death to Testability](https://testing.googleblog.com/2008/12/static-methods-are-death-to-testability.html) and [Writing Testable Code](https://testing.googleblog.com/2008/08/by-miko-hevery-so-you-decided-to.html)

## Flaw #2: Brittle Global State & Singleton

### Rule #1: Avoid Global States

#### Warning Signs

* Adding or using static fields or static methods
* Adding or using registries
* Using `System.currentTimeMillis()`, `new Date()` or `Math.random()`

Global states are a bad idea because they make the code hard to understand and reason about.

Moreover, they cause problems for testing because global states are persistent throughout the lifetime of a JVM instance. In unit testing, we have one JVM instance to run all tests (instead one JVM for one test, for performance reason). Thus the global states are persistent from tests to tests. Now, if some tests expect the global states to be in state A, while some other tests expect the global states to be in state B. In this scenario, you cannot run the tests in parallel, otherwise your tests will become flaky (sometimes pass, sometimes fail) because the order of tests matters.

*Sources*: [Writing Testable Code](https://testing.googleblog.com/2008/08/by-miko-hevery-so-you-decided-to.html).

### Rule #2: Avoid Singleton Pattern

Singleton Pattern, despite being a well-know design patter, are global states in sheep's clothing. They have **globally** accessible `getInstance()` method and a private singleton object (which is the global state).

Another problem about Singleton Pattern is that they are globally accessible, thus any method can access them inside the code without explicitly declaring in its API. In other words, the API lied about its dependency!

**Note** singletons (with lowercase s) are valid and sometimes very useful, which you can enforce their singleton identities by enforcing their constructors are called only once in the application. However, Singleton Pattern (with uppercase S) is almost always causing undesirable global states, which you should avoid. Only few examples of Singleton Pattern, including Constants and Logging, are acceptable since they don't affect the application logic.

*Sources*: [Singletons are Pathological Liars](https://testing.googleblog.com/2008/08/by-miko-hevery-so-you-join-new-project.html) and [Writing Testable Code](https://testing.googleblog.com/2008/08/by-miko-hevery-so-you-decided-to.html)

## Flaw #3: Dig Into Collaborators

### Rule #1: Don't Look for Things. Ask for Things.

#### Warning Signs

* Objects are passed in but never used directly (only used to get access to other objects)
* [Law of Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter) violation: method call chain walks an object graph with more than one dot. (e.g. `a.getB().getC().doSomething()`)

The Law of Demeter can be summarized as "only talk to your **immediate** friends".

Previously we mentioned the importance of dependency injection for creating seams in [Flaw #1: no seams for isolating the class under test](#flaw-1-no-seams-for-isolating-the-class-under-test). Here we emphasize on injecting only those **direct dependency**.

Let's use an example to illustrate why **indirect** dependency hurts testability:

Say you have an Authenticator class which needs a `Config` object, do you pass in a path to the configuration file, or just pass in a `Config` object? Since what you need is a `Config` object, not a file path, an example of indirect dependency will be you pass in a path to the configuration file, and read a `Config` object from the path. To test Authenticator, you have to first write some configuration to a file, pass the file path to Authenticator, and let it read a `Config` itself. Versus you can directly create a `Config` object and pass it to Authenticator using direct dependency.

*Sources*: [Writing Testable Code](https://testing.googleblog.com/2008/08/by-miko-hevery-so-you-decided-to.html).

## Flaw #4: Class Does Too Much

To extreme, it becomes an anti-pattern, [God Object](https://en.wikipedia.org/wiki/God_object).

### Rule #1: Avoid Mixing of Concerns

#### Warning Signs:

* summing up what the class does includes the word "and"
* class has fields that are only used in some methods (objects hiding inside)
* class has static methods that only operate on parameters (methods should be instance methods of one of the parameters)

These classes are hard to test since there are multiple objects hiding inside of them and as a result you are testing all of the objects at once.

[Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle) also requires you to have classes with one concern (one reason to change), because these classes are simpler to understand what they are doing and easier to unit test.

*Sources*: [Where Have all the "new" Operators Gone?](https://testing.googleblog.com/2008/09/where-have-all-new-operators-gone.html) and [Writing Testable Code](https://testing.googleblog.com/2008/08/by-miko-hevery-so-you-decided-to.html)

### Rule #2: Favor Polymorphism Over Conditionals

#### Warning Signs:

* Class has the same `switch` or `if` conditions in many places

For a detailed code example, please refer to this [Stack Overflow answer](https://stackoverflow.com/a/234491/3522482).

From the answer, if you have one switch statement based on an internal field you probably have the same switch in multiple places. This causes problems when you add a new case as you have to update all the switch statements.

By using polymorphism, you get the same functionality and because a new case is a new class you don't have to search your code for things that need to be updated. It is all isolated for each class.

This closely follows with [Open Closed Principle](https://en.wikipedia.org/wiki/Open/closed_principle) because you can ship this abstract parent class (**closed for modification**) as part of your binary library, but people can still extend the functionality by adding new child classes (**open for extension**).

*Sources*: [Writing Testable Code](https://testing.googleblog.com/2008/08/by-miko-hevery-so-you-decided-to.html).

# Concluding Notes

This article includes ten rules that can help you understand some key concepts, such as seams, dependency injection, global states, singletons and Singleton. Also, I hope you can apply these rules into practice, like writing a program with these rules in mind or reviewing some code your wrote before and see whether you can improve its testability, so you can benefit from the things you learn in this article.

</div>
