<frontmatter>
  title: "Advanced Java: Reflections"
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

# Advanced Java: Reflections

Authors: [Jeremy Goh](https://github.com/MightyCupcakes), [Yong Zhi Yuan](https://github.com/Zhiyuan-Amos)

## Reflection

### What is Java reflections

Reflection is the ability of a computer program to examine, inspect and modify its own behaviour at runtime. In particular, reflections in Java allows the inspection of classes, methods and fields during runtime, without having any knowledge of it during compile time.

With Java reflections, you can:
1. Access private fields and methods of a class without having to modify the visibility modifier of the class itself. This is very useful if you are interested to implement test cases for private methods.
1. Create new instance of existing objects, invoke methods and change values of fields of existing objects.

### The Basics of Reflections

Before getting started with reflections in Java, it is important to realize that a class is also an object. From the [Java Class API](https://docs.oracle.com/javase/9/docs/api/java/lang/Class.html), we see that `Class` is a subclass of `Object`. Every unique `Object` is assigned an immutable `Class` object by the JVM. This immutable `Class` object is fundamentally different from *instances* of a class. The class object itself holds information such as its name and the package it resides in while an instance of a class holds the instanced values and methods as defined in the class.

Take for example the following class:

```java
public class Student {

  private final String name;
  private final String gender;

  public Student(String name, String gender) {
    this.name = name;
    this.gender = gender;
  }

  // Other methods here...
}
```

An instance of `Student` class can be created as usual using the `new` keyword:

```java
Student john = new Student("John Doe", "Male");
```

However, it is also possible to get information about the `Student` class itself because the class itself is an `Object`:

```java
Class<Student> studentClass = Student.class;
```

This means that you can store any `Class` object in any data structure for future retrieval. This is the main entry point for Java's reflections. You can now get the name of the class, create new instances of the class, observe its public/private fields - the possibilities are endless!

### Getting Started

There are many webpages dedicated to explaining the details of reflections in Java; so this will not repeat what is being made readily available on the web. One good place to start is this [article by JavaWorld](http://www.javaworld.com/article/2077015/java-se/take-an-in-depth-look-at-the-java-reflection-api.html).

One important point is that while Java reflections are powerful, its implementations are not very straightforward. There are however some libraries out there such as the [Google's Guava library](https://github.com/google/guava/wiki/ReflectionExplained) which contains many utility methods and classes that makes our life easier.

### Applications

#### Accessing private fields

A good example of reflections is to get a private field of another class. While this should optimally be solved by modifying the field visibility to `protected` or `public`, sometimes it is not possible to do so because you might not have any access to the code (for example codes in libraries or frameworks).

For the sake of simplicity, let us use a example of a simple `Animal` class. The class definition can look like this.

```java
public class Animal {

  private int age;

  public Animal() {
    age = 0;
  }
}
```

And let us assume that you, for some reason, cannot modify this code. But you are interested in making a new class `Sheep` that extends `Animal` and do something when his age reaches certain threshold. But the annoying thing is that somebody decided that it is a good idea to make the age value `private` (and without a getter method!) instead of `protected` in a top-level class such as this! So you cannot access the age of your `Sheep` even though it is an `Animal`.

This of course can be solved by Reflection as follows:

```java
public class Sheep extends Animal {
  public Sheep() {
    super();
  }

  /**
   * A sheep begins to produce wool after it's a year old!
   */
  public boolean isProducingWool() {
    return getAge() > 1;
  }

  private int getAge() {
    // Since class is a reserved keyword, we use clazz in variable names
    Class<Animal> animalClazz = Animal.class;

    try {
      Field f = animalClazz.getDeclaredField("age");
      f.setAccessible(true);
      return (int) f.get(this);
    } catch (IllegalAccessException | NoSuchFieldException e) {
      // perform error handling
    }
  }
}
```

And there you have it! What `Sheep` is really doing is to examine itself at runtime in order to obtain the `age` field inherited from `Animal`. This technique can be used in test cases to access private fields and methods in the class under test without modifying the visibility modifiers of the fields and methods in the class itself.

You may notice that the `Sheep#getAge()` method sets the age `Field` object to be accessible and might wonder the implications. Fret not! The `Field#getDeclaredField()` actually returns a new `Field` instance - so you're just setting that particular local `Field` instance to be accessible, not the actual `age` field itself. You can read more about it in this [StackOverflow question](http://stackoverflow.com/questions/10638826/java-reflection-impact-of-setaccessibletrue).

Do take note that two exceptions need to be handled when accessing fields: 
1. `IllegalAccessException`, which occurs if the field is private and you did not set the accessibility modifier to be true (e.g. `f.setAccessible(true)`).
1. `NoSuchFieldException`, which occurs if the field with the specified name (e.g. `age`) does not exist.

#### Updating private fields

Suppose that you need to write tests for `Sheep`. As part of setting up the test, you need to create a sheep with age = 20. Suppose that the age of the sheep is updated automatically as time passes, whereby the age increases by 1 after every minute. A naive way of creating a sheep with age = 20 is to simply wait for 20 minutes before performing the test:

```java
@Test
public void foo() throws Exception {
  Sheep sheep = new Sheep();
  TimeUnit.MINUTES.sleep(20);
  // perform test
}
```

Alternatively, a much simpler and efficient way to perform this test is to set a value to the private field using Reflection:

```java
@Test
public void foo() throws Exception {
  Sheep sheep = new Sheep();
  Field field = Animal.class.getDeclaredField("age");
  field.setAccessible(true);
  field.set(sheep, 20);
  // perform test
}
```

#### Testing private methods

Suppose you want to perform a unit test for the method `getAge()`. However, you are only able to indirectly do so by testing `isProducingWool()`. This is not good as we are not able to directly verify the age of a sheep. However, with the help of Reflection, we can now test private methods.

```java
@Test
public void foo() throws Exception {
  Sheep sheep = new Sheep();
  Method method = Sheep.class.getDeclaredMethod("getAge");
  method.setAccessible(true);
  int age = (int) method.invoke(sheep);
  // verify age
}
```

#### A more advanced application

You might have learnt from your Software Engineering module that the Observer pattern can be used for objects that are interested to get notified if the state of another object is changed. The Observer pattern is useful because you can avoid creating bidirectional dependencies between two unrelated objects that have no business talking to each other while allowing the objects to be notified of any changes in another object.

One prime example of the implementation of the Observer pattern is the Google Events bus used in [AddressBook Level 4](https://github.com/se-edu/addressbook-level4/). The event bus uses reflections to observe all registered objects via `register` method for methods annotated with the `Subscribe` annotation.

An example implementation (not the actual) of the `Subscribe` annotation might be:

```java
@Retention(RetentionPolicy.RUNTIME) // Retain this annotation at runtime!
@Target(ElementType.METHOD) // Only can be applied to methods!
public @interface Subscribe { }
```

And that is it! Important parts of the code to note are the first two lines before the declaration. The first line tells Java that this annotation must not be discarded during compile time so it will be available during runtime. The retention policy is there because some annotation do not mean anything after compilation (such as `Override` and `SuppressWarnings`), so it does not make sense to keep the annotation after compiling.

The second line just means that this annotation can be applied to methods only.

And the more important part is how the subscriber registry finds all its subscribing methods. The first step is to register a class as an event handler and an example of the code is like so:

```java
public class Sheep extends Animal {

  private static final EventsCenter EVENTS_BUS = EventsCenter.getInstance();
  
  public Sheep() {
    super();
    EVENTS_BUS.register(this.getClass());
  }

  @Subscribe
  public void handleWeatherChangeEvent(WeatherChangeEvent event) {
    if (event.weather == Weather.RAIN) {
      hide();
    }
  }
  ...
}
```

An example implementation of the `EventsCenter` (with a lot of details left out for simplicity) is like so:

```java
public void register(Class<?> clazz) {
  findAllEventHandlersInClass(clazz);
}

public void findAllEventHandlersInClass(Class<?> clazz) {
  // TypeToken class is provided by Google Guava reflection library
  Set<? extends Class<?>> supertypes = TypeToken.of(clazz).getTypes().rawTypes();

  for (Class<?> supertype : supertypes) {
    for (Method method : supertype.getDeclaredMethods()) {
      if (method.isAnnotationPresent(Subscribe.class) {
        registerSubscriber(method);
      }
    }
  }
}

```

The first line of the `findAllEventHandlersInClass` method finds all classes and its parent classes of the registered class and converts it to a set. That is if you registered `Sheep extends Animal` as an event handler to the method, both `Sheep` and `Animal` will be captured by the first line. The following lines will then examine all their methods (during runtime!) for the `Subscribe` annotation and register the method so that it will receive the specified event when it is fired. Of course this implementation leaves out a lot of details but you get the idea of how Java reflections works.

### Disadvantages of reflections

While Java reflections are powerful, you should not immediately jump on the reflections ship. This is because there are some drawbacks whenever you use reflections in your project. The following are some points you should consider before using Java reflections:

* Reflections convert a compile-time error to a potentially destructive run-time error.

  Compile time errors are easy to catch. Whenever you compile your code, the compiler cleverly spots any error you missed and points it out (along with line number and other useful information) to you before quitting. But by using reflections, you are bypassing these checks because there is no way to check such errors during compile time. These uncaught errors may cause your program to fail during runtime instead, turning into runtime errors.

  For example you might have come across this problem where your program crashed and you get a `NullPointerException` error in your crash log. As you might have experienced already, runtime errors are more troublesome in that they are harder to catch and debug. They might even bring your whole software under the water with it by crashing the whole thing.

* Reflections are harder to understand and harder to debug

  There is a reason why the topic of reflections is placed under the advanced section. Codes using reflections are fundamentally harder to understand. As mentioned above, it is also harder to debug when the classes might not even be there during compile time. This makes your code very hard to maintain.

* Poor performance

  Since reflections resolve types dynamically, performance suffers. This is usually not an issue with small software but you might want to keep it in mind if you want to scale up.

* Bad Security

  The second example demostrated a way to access the private fields of a class using reflections. This should be very concerning if your software deals with sensitive information because other classes can access fields that they are not supposed to.

* Indication of bad class design

  Having to use reflection in order to bypass a class' encapsulation is usually indicative of an API design problem. We can remove the usage of Reflection in the examples given [above](#accessing-private-fields) by adding a getter and setter method for `age`. See this [post](https://stackoverflow.com/questions/34571/how-do-i-test-a-private-function-or-a-class-that-has-private-methods-fields-or/34658#34658) for further discussion. In this scenario whereby we cannot add a getter and setter method for `age`, we should create our own implementation of `Animal` class with the getter and setter methods.

### Further Resources for reflections

* Introductions to Java reflections with some explanation

  [http://www.javaworld.com/article/2077015/java-se/take-an-in-depth-look-at-the-java-reflection-api.html](http://www.javaworld.com/article/2077015/java-se/take-an-in-depth-look-at-the-java-reflection-api.html)
  [http://www.journaldev.com/1789/java-reflection-example-tutorial](http://www.journaldev.com/1789/java-reflection-example-tutorial)

* A short but precise overview of Java reflections

  [http://www.oracle.com/technetwork/articles/java/javareflection-1536171.html](http://www.oracle.com/technetwork/articles/java/javareflection-1536171.html)

* Google's Guava reflection library provides some utility methods and classes 

  [https://github.com/google/guava/wiki/ReflectionExplained](https://github.com/google/guava/wiki/ReflectionExplained)



</div>
