<frontmatter>
  title: Null Safety in Kotlin
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Null Safety in Kotlin

Author: [Pan Haozhe](https://github.com/Haozhe321)

<box id="article-toc">

* [What is Null Safety?‎](#what-is-null-safety)
    * [NullPointerException‎](#nullpointerexception)
* [How Does Kotlin Handle Null Safety?‎](#how-does-kotlin-handle-null-safety)
    * [Nullable and Non-Nullable Type‎](#nullable-and-non-nullable-type)
    * [Safety Operators in Kotlin‎](#safety-operators-in-kotlin)
        * [Safe Call Operator‎](#safe-call-operator)
        * [Elvis Operators‎](#elvis-operators)
        * [Not-Null Assertion Operator‎](#not-null-assertion-operator)
* [Summary‎](#summary)
* [Learning Resources‎](#learning-resources)
</box>

>“I call it my billion-dollar mistake. It was the invention of the null reference…My goal was to ensure that all use of references should be absolutely safe, with **checking performed automatically by the compiler**. But I couldn't resist the temptation to put in a null reference, simply because it was so easy to implement.”  
-Tony Hoare

This document explains Kotlin's null safety feature. For an overview of Kotlin, see [here](kotlin.html).

# What is Null Safety?
_Null Safety_ (or _void safety_) is the guarantee that no object reference will have a `null` value.
In object-oriented languages, access to objects is achieved through references. A typical function call is of the form `object.func()`; `object` denotes a reference to a certain object, and `func` denotes a function call. At execution time, the reference to `object` can be `void`, leading to run-time exceptions (In the case of Java, a NullPointerException) and often abnormal termination of the program.

## NullPointerException
 When developing Android applications in Java, [NullPointerException (NPE)](https://docs.oracle.com/javase/9/docs/api/java/lang/NullPointerException.html) was a big problem. In fact, About [one third of app crashes can be attributed to NPE](https://image.slidesharecdn.com/droidcon-bugsense-130408170720-phpapp01/95/droid-con-bugsense-16-638.jpg?cb=1365440918). To see how it happens, let's take a look at the piece of Java code below:

```java
String a = null;
if(a.length > 5) {
    //do something
}
```
When the above code is run, an NPE will be thrown on line 2 because a `null` object has no methods. To prevent an object from taking on a `null` value, programmers typically resort to doing additional checks like this:
```java
String a = null;
if(a != null && a.length > 5) {
        //do something
}
```
And of course, that's all fine, until we want to do something more complex. Say Bob belongs to a department, and we want to get the name of the department manager. That will look like this:
```java
String managerName = bob.department.manager.name;
```

Because each variable can be `null`, to prevent the NPE we put the code in the following code block:
```java
if(bob != null) {
    Department department = bob.department;
    if(department != null) {
        Employee manager = department.manager;
        if(manager != null) {
            String name = manager.name;
            if(name != null) {
                //do something
            }
        }
    }
}
```
The deep-nested `if` statement reduces readability of our code.

The other way is to use [Java Optionals](https://www.oracle.com/technetwork/articles/java/java8-optional-2175753.html).
For the first example above, we can do
``` java
a.ifPresent(this::doSomething);
```

And for the deeply-nested `if` statements, the verbosity can be reduced with
``` java
bob.map(Person::getDepartment)
    .map(Person::getManager)
    .map(Person::getName)
    .ifPresent(Person::doSomething);
```
`map()` is a method in Java Optionals class that applies the function inside the parentheses to the object that is calling it. If the object is not present, it will return an empty Optional.  

Let's see how Kotlin deals with this issue while maintaining a simple and readable syntax.

# How Does Kotlin Handle Null Safety?
## Nullable and Non-Nullable Type
In Kotlin, a type can be _nullable_ or _non-nullable_, determined by the presence of a `?`. For example, an object of type `String` is non-nullable, while an object of type `String?` is nullable.  

As the compiler catches `null` assignments to non-nullable objects, the following would result in compilation error.
```Kotlin
var firstString: String = "hello world"
firstString = null //compilation error
```
In comparison, the following assignment to a nullable type is allowed
```Kotlin
var secondString: String? = "hello world"
secondString = null //okay
```

In the first case, we can safely call `firstString.length` without having to worry about a NPE because `firstString` can never be `null`.   
In the second case, `secondString` can potentially be `null`, so `secondString.length` will result in a compilation error as the compiler see the danger of such statement and blocks it early.


## Safety Operators in Kotlin
Although non-nullable type is a strong feature in Kotlin, the [interoperability](https://kotlinlang.org/docs/reference/java-interop.html) with Java means that we have to use variables as nullable type sometimes. In the previous section, we seem to have hit an obstacle as the compiler blocks the call to `secondString.length`. In this section we look at some ways of overcoming this problem.
### Safe Call Operator
Represented by `?.`, the safe call operator is used in this way  
```kotlin
secondString?.length
```
This returns the length of `secondString` if `secondString` is not `null`, and `null` otherwise.

Now we can chain like this
```Kotlin
bob?.department?.manager?.name
```
This chain will return `null` if any of the variables inside the chain is `null`.


### Elvis Operators
Represented by `?:`, the Elvis operator is used in this way
```kotlin
val length = secondString?.length ?: -1
```
If the expression to the left of `?:` is not null, the Elvis operator (`?.`) will return it as it is; else it will return a default value supplied (-1 in this case).

We also notice the use of safe call operator together with Elvis operator in the same statement.

But the Elvis operator is more powerful than this. `return` and `throw` statements are legitimate default values on the right side of the Elvis operator. So you can define your own error message to aid debugging. For example:
```kotlin
fun myFunc(node: Node): String? {
    val parent = node.getParent() ?: return null
    val name = node.getName() ?: throw IllegalArgumentException("Name expected")
    // ...
}
```
Doing so like this can help programmers to check for function arguments before carrying on with the required computation.

At this point you may ask, "What if I still want my NPE?"


### Not-Null Assertion Operator
Represented by `!!`, the not-null assertion operator is used in this way
```kotlin
val stringLength = secondString!!.length
```
The operator converts any value to a non-nullable type and throws an exception if the value is null. In the above example, `stringLength` will be assigned the length of `secondString` if `secondString` is not `null`; if secondString is `null`, a NPE is thrown. Kotlin tries to reduce the number of NPEs thrown as it is a run-time exception that is difficult to debug, in addition to creating so many app crashes. Hence NPEs in Kotlin are explicitly asked for.


# Summary
1. Kotlin increases null safety of programs because some of the work required to ensure null safety is offloaded from the programmer to the compiler, which is less error prone.
2. Null Safety is enforced by the Kotlin language. This is better compared to Java Optionals which is a Class and not a language construct like Kotlin's null-safety system.
3. If you expect your object to **not** take on a `null` value, make it a non-nullable type!
4. Even if you make your object a nullable type, Kotlin handles it better than Java because it can help to prevent NPE. An generic NPE is hard to debug; in Kotlin a descriptive message could be given to make debugging easier (with the help of Elvis operator).


# Learning Resources
* [Null Safety in Kotlin](https://kotlinlang.org/docs/reference/null-safety.html)
* [Comprehensive Guide to Null Safety in Kotlin](https://www.baeldung.com/kotlin-null-safety)
* [The Billion Dollar Mistake](https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare)

</div>
