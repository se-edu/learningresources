# Advanced Java: Reflections

Authors: [Jeremy Goh](https://github.com/MightyCupcakes)

## Reflection

### What is Java reflections

Reflection is the ability of a computer program to examine, inspect and modify its own behaviour at runtime. In particular, reflections in Java allows the inspection of classes, methods and fields during runtime, without having any knowledge of it during compile time.

With Java reflections, you can:
1. Access private fields and methods of a class without having to modify the visibility modifier of the class itself. This is very useful if you are interested to implement test cases for private methods.
1. Create new instance of existing objects, invoke methods and change values of fields of existing objects.

### The Basics of Reflections

Before getting started with reflections in Java, it is important to realize that classes is also an object. Every unique `Object` is assigned an immutable `Class` object by the JVM. This immutable `Class` object is fundamentally different from *instances* of a class. The class object itself holds information about its name, the package it resides in and other important information while an instance of a class holds the instanced values and methods as defined in the class.

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

An instance of a class can be created as usual using the `new` keyword

```java
Student john = new Student("John Doe", "Male");
```

However, it is also possible to get information about the class itself because the class itself is an `Object` like so:

```java
Class<Student> studentClass = Student.class;
```

It also means that you can store any `Class` object in any data structure for future retrieval. This is the main entry point for Java's reflections. You can now get the name of the class, create new instances of the class, observe its public/private fields - the possibilities are endless!

### Getting Started

There are many webpages dedicated to explaining the details of reflections in Java; so this will not repeat what is being made readily available on the web. One good place to start is [http://www.javaworld.com/article/2077015/java-se/take-an-in-depth-look-at-the-java-reflection-api.html](http://www.javaworld.com/article/2077015/java-se/take-an-in-depth-look-at-the-java-reflection-api.html)

One important point is that while Java reflections are powerful, its implementations are not very straightforward. There are however some libraries out there to make life easier such as the [Google's Guava library](https://github.com/google/guava/wiki/ReflectionExplained) which contains many utility methods and classes to make your life easier.

### Applications

#### Accessing private fields

A good example of reflections is to get a private field of another class. While this should optimally be solved by modifying the field visibility to `protected` or `public`, sometimes it is not possible to do so because you might not have any access to the code (for example codes in libraries or frameworks).

For the sake of simplicity, let us use a example of a simple `Animal` class. The class definition can look like this.

```java

public class Animal {

    private int age;
    private boolean isAlive;
    public final String name;

    public Animal(String nameOfAnimal) {
        name = nameOfAnimal;
        age = 0;
        isAlive = true;
    }

    public void onUpdate() {
        age++;

        if (age > 100) {
            setDead();
        }
    }

    // Other methods here...
}
```

And let us assume that you, for some reason, cannot modify this code. But you are interested in making a new class `Sheep` that extends `Animal` and do something when his age reaches certain threshold. But the annoying thing is that somebody decided that it is a good idea to make the age value `private` instead of `protected` in a top-level class such as this! So you cannot access the age of your `Sheep` even though it is an `Animal`.

This of course can be solved by reflections as follows:

```java

public class Sheep extends Animal {
    private boolean hasWool;

    public Sheep(String animalName) {
        super(animalName);
        hasWool = false;
    }

    @Override
    public void onUpdate() {
        super.onUpdate();

        int age = getPrivateAge();

        if (age > 20) {
            setHasWool();
        }
    }

    private int getPrivateAge() {
        // Since class is a reserved keyword, we use clazz in variable names
        Class<Animal> animalClazz = Animal.class;

        try {
            Field f = animalClazz.getDeclaredField("age");
            f.setAccessible(true);
            return (int) f.get(this);
        } catch (Exception e) {
            e.printStackTrace();
        }

        return 0;
    }

    // Other methods here...
}
```

And there you have it! What `Sheep` is really doing is to examine itself at runtime in order to obtain the `age` field inherited from `Animal`. If it cannot, it assumes its `age` is zero. This technique can be used in test cases to access private fields and methods in the class under test without modifying the behaviour of the class itself.

You may notice that the `Sheep#getPrivateAge()` method sets the age `Field` object to be accessible and might wonder the implications. Fret not! The `Field#getDeclaredField()` actually returns a new `Field` instance - so you're just setting that particular local `Field` instance to be accessible, not the actual `age` field itself. You can read more about it in this [StackOverflow question](http://stackoverflow.com/questions/10638826/java-reflection-impact-of-setaccessibletrue).

#### A more advanced application

You might have learnt from your Software Engineering module that the Observer pattern can be used for objects that are interested to get notified if the state of another object is changed. The Observer pattern is useful because you can avoid creating bidirectional dependencies between two unrelated objects that have no business talking to each other while allowing the objects to be notified of any changes in another object.

One prime example of the implementation of the Observer pattern is the Google Events bus used in [AddressBook Level 4](https://github.com/se-edu/addressbook-level4/).The event bus uses reflections to observe all registered objects via `register` method for methods annotated with the `Subscribe` annotation.

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

    public final String name;

    public Sheep(String nameOfSheep) {
        name = nameOfSheep;
        EVENTS_BUS.register(this);
    }

    @Subscribe
    public void handleWeatherChangeEvent(WeatherChangeEvent event) {
        if (event.weather == Weather.RAIN) {
            hide();
        }
    }

    // Other methods here...
    
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

The first line of the `findAllEventHandlersInClass` method finds all classes and its parent classes of the registered class and converts it to a set. That is if you registered `Sheep extends Animal` as an event handler to the method, both `Sheep` and `Animal` will be captured by the first line. The following lines will then examine all their methods (during runtime!) for the `Subscribe` annotation and register the method so that it will receive the specified event when it is fired. Of course this implementation leaves out a lot of details but you get the idea of how java reflections works.

### Disadvantages of reflections

While Java reflections are powerful, you should not immediately jump on the reflections ship. This is because there are some drawbacks whenever you use reflections in your project. The following are some points you should consider before using Java reflections:

* Reflections convert a compile-time error to a potentially destructive run-time error.

  Compile time errors are easy to catch. Whenever you compile your code, the compiler cleverly spots any error you missed and points it out (along with line number and other useful information) to you before quitting. But by using reflections, you are bypassing these checks because there is no way to check such errors during compile time. These  uncaught errors may cause your program to fail during runtime instead, turning into runtime errors.

  For example you might have come across this problem where your program crashed and you get a `NullPointerException` error in your crash log. As you might have experienced already, runtime errors are more troublesome in that they are harder to catch and debug. They might even bring your whole software under the water with it by crashing the whole thing.

* Reflections are harder to understand and harder to debug

  There is a reason why the topic of reflections is placed under the advanced section. Codes using reflections are fundamentally harder to understand. As mentioned above, it is also harder to debug when the classes might not even be there during compile time. This makes your code very hard to maintain.

* Poor performance

  Since reflections resolve types dynamically, performance suffers. This is usually not an issue with small software but you might want to keep it in mind if you want to scale up.

* Bad Security

  The second example demostrated a way to access the private fields of a class using reflections. This should be very concerning if your software deals with sensitive information because other classes can access fields that they are not supposed to.

### Further Resources for reflections

* Introductions to Java reflections with some explanation

  [http://www.javaworld.com/article/2077015/java-se/take-an-in-depth-look-at-the-java-reflection-api.html](http://www.javaworld.com/article/2077015/java-se/take-an-in-depth-look-at-the-java-reflection-api.html)
  [http://www.journaldev.com/1789/java-reflection-example-tutorial](http://www.journaldev.com/1789/java-reflection-example-tutorial)

* A short but precise overview of Java reflections

  [http://www.oracle.com/technetwork/articles/java/javareflection-1536171.html](http://www.oracle.com/technetwork/articles/java/javareflection-1536171.html)

* Google's Guava reflection library provides some utility methods and classes 

  [https://github.com/google/guava/wiki/ReflectionExplained](https://github.com/google/guava/wiki/ReflectionExplained)


