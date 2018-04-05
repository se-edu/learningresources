# Null Safety in Kotlin

Author: [Pan Haozhe](https://github.com/Haozhe321)

# Overview
Another [part](https://github.com/se-edu/learningresources/blob/master/contents/kotlin/kotlin.md) of this series on Kotlin gives an introduction to Kotlin. This piece aims to discuss the null safety feature in Kotlin, and what was previously missing in Java.


# The Billion Dollar Mistake
“I call it my billion-dollar mistake. It was the invention of the null reference…My goal was to ensure that all use of references should be absolutely safe, with **checking performed automatically by the compiler**. But I couldn't resist the temptation to put in a null reference, simply because it was so easy to implement.”  
-Tony Hoare  


# NullPointerException
 When developing Android applications in Java, [NullPointerException(NPE)](https://docs.oracle.com/javase/9/docs/api/java/lang/NullPointerException.html) was a big problem. In fact, About [one third of app crashes can be attributed to NPE](https://image.slidesharecdn.com/droidcon-bugsense-130408170720-phpapp01/95/droid-con-bugsense-16-638.jpg?cb=1365440918). To see how it happens, let's take a look at the piece of Java code below:

```java
String a = null;
if(a.length > 5) {
    //do something
}
```
When the above code is ran, a NPE will be thrown on line 2 because a `null` object has no methods. To prevent an object from taking on a `null` value, programmers typically resort to doing additional checks like this:
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
The deep-nested `if` statement adds to the verbosity of our code.

The other way is to use [Java Optionals](http://www.oracle.com/technetwork/articles/java/java8-optional-2175753.html).
For the first example above, we can do
``` java
a.ifPresent(this::doSomething);
```

And for the deeply-nested if statements, the verbosity can be reduced with
``` java
bob.map(Person::getDepartment)
    .map(Person::getManager)
    .map(Person::getName)
    .ifPresent(Person::doSomething);
```

Let's see how Kotlin deals with this issue while maintaining a simple and readable syntax.

# Nullable and Non-nullable type
In Kotlin, a type can be nullable or non-nullable, determined by the presence of a `?`. For example, an object of type `String` is non-nullable, while an object of type `String?` is nullable.  

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


# Operators in Kotlin
Although non-nullable type is a strong feature in Kotlin, the [interoperability](https://kotlinlang.org/docs/reference/java-interop.html) with Java means that we have to use variables as nullable type sometimes. In the previous section, we seem to have hit an obstacle as the compiler blocks the call to `secondString.length`. In this section we look at some ways of overcoming this problem.
## Safe call operator
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


## Elvis Operators
Represented by `?:`, the Elvis operator is used in this way
```kotlin
val length = secondString?.length ?: -1
```
If the expression to the left of `?:` is not null, the Elvis operator (`?.`) will return it as it is; else it will return a default value supplied (-1 in this case).

We also notice the use of safe call operator together with Elvis operator in the same statement.

But the Elvis operator is more powerful than this. `return` and `throw` statements are legitimate default values on the right side of the Elvis operator. For example:
```kotlin
fun myFunc(node: Node): String? {
    val parent = node.getParent() ?: return null
    val name = node.getName() ?: throw IllegalArgumentException("Name expected")
    // ...
}
```
Doing so like this can help programmers to check for function arguments before carrying on with the required computation.

At this point you may ask, "What if I still want my NPE?"


## Not-null assertion operator
Represented by `!!`, the not-null assertion operator is used in this way
```kotlin
val stringLength = secondString!!.length
```
The operator converts any value to a non-null type and throws an exception if the value is null. In the above example, `stringLength` will be assigned the length of `secondString` if `secondString` is not `null`; if secondString is `null`, a NPE is thrown. Kotlin tries to reduce the number of NPE as an NPE is difficult to debug a generic NPE, and also it creates so many app crashes. Hence NPEs in Kotlin are explicitly asked for.


# Summary
1. Kotlin makes your applications safer by allocating more work to the complier, instead of failing in the hands of the users.
2. If you expect your object to **not** take on a `null` value, make it a non-null type!
3. Even if you make your object a nullable type, Kotlin handles it better than Java because it can help to prevent NPE. An generic NPE is hard to debug; in Kotlin a descriptive message could be given to make debugging easier(with the help of Elvis operator).


# Learning resources
* [Null Safety in Kotlin](https://kotlinlang.org/docs/reference/null-safety.html)
* [Comprehensive Guide to Null Safety in Kotlin](http://www.baeldung.com/kotlin-null-safety)
* [The Billion Dollar Mistake](https://www.infoq.com/presentations/Null-References-The-Billion-Dollar-Mistake-Tony-Hoare)
