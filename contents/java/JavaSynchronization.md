<frontmatter>
  title: Java Synchronization
  footer: footer.md
  head: head.md
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Java Synchronization

Authors: [Boxin](https://github.com/boxin-yang)

# Introduction

Java Synchronization is the Java implementation of [Monitor][1]. The Monitor is used with [parallel threads][2] to ensure [mutual exclusion][3] in [critical section][4]. It is a mechanism to ensure [thread safety][5] when parallel threads are accessing shared data of an object.

Java builds Monitor into [Java Object][6] and each object is itself a Monitor. Although Java has also implemented other mechanisms such as [Semaphore][7] in Java 5, Java Monitor is more widely used since it is supported in the first version of Java. In this tutorial, we will go through Java Synchronization in terms of basic syntax, useful features and best practice.

# Basic Syntax

Java Synchronization is done by using keyword **synchronized**. It is commonly used in two forms: *synchronized statement* and *synchronized method*. We will use example codes to illustrate how synchronized keyword is used.

## synchronized statement

Let us look at the following code snippet to understand synchronized statement:

```
synchronized(object) {
	// critical section
}
```

The **synchronized** keyword takes in an **object** as an argument and uses brackets to enclose the **critical section** that accesses object data. In the code above, the **object** argument is the object to be used as the Monitor. When **critical section** is executed, the **object Monitor** locks all data of the object and only the **critical section**in the synchronized block can access data of the **object**. When the execution of **critical section** finishes on a certain thread, the thread gives up the **object Monitor** lock and other threads can access the data of **object**. Therefore, as long as **critical section** is included in a synchronized block of a **object Monitor**, data of the object is guaranteed to be thread safe.

## synchronized method

Another common way to use **synchronized** functionality is called **synchronized method**. The following code snippet is an example.

```
public synchronized void foo() {
	// method code
}
```

In this example, **synchronized** is used as a method modifier to indicate that the method is synchronized. In the case of a synchronized method, all the content in the method is considered as **critical section** and the Monitor object used is dependent on the method. If the method is an instance method, then the Monitor object used is **this** instance. If the method is a class method, then the Monitor object used is a special class object initialized during run time for each class. The equivalence of the above code is:

```
public void foo() {
	synchronized(this) {
		// method code
	}
}
```

# More features of Java synchronization

Besides synchronized keyword to guard a critical section, another important feature of Java Synchronization is **wait()** and **notify()**.

## wait() and notifyAll()
In a synchronized block, sometimes some conditions are required for execution. If such conditions are not met, wait() method can be used to give up the Monitor object lock and put the current thread into a * callback queue** once the conditions are met. Let us use the following code as an example.

```
public synchronized void eatChicken() {
	while (!hasChicken) {
		wait();
	}
	eat();
	hasChicken = false;
}

public synchronized void cookChicken() {
	hasChicken = true;
	notifyAll();
}
```

In method eatChicken, the condition of hasChicken must be true before eat() can take place. However, instead of terminating the method, the program can call wait() and then this thread is put into a * callback queue**. When some other threads change the conditions, in this case, if cookChicken() method produces chicken and the condition for eatChicken() might be changed, cookChicken() can call notifyAll(). notifyALl() wakes up one by one all the threads in the * callback queue** associated with the Monitor object used to by coooChicken(). In this case, eatChicken() is woken up and eatChicken() gains Monitor object lock and starts execution from wait() onwards. This is basically how wait() and notifyAll() work in Java. However, here are some points to take notes to use them correctly.

### Understand which object is used as Monitor
As described from the very beginning, all the synchronized feature is associated with a Monitor object as a lock. This is also true for wait() and notifyAll(). The Monitor object associated with wait() and notifyAll() is the Monitor object associated with the synchronized block that calls wait() and notifyAll().
When wait() is called, the current thread pull is put into the callback queue of the Monitor object of the synchronized block. When notifyAll() is called, only threads waiting in the Monitor object queue associated with notifyAll() will be notified. Therefore, in the above example of eatChicken() and cookChick(), the wait() and notifyAll() will only work if both eatChicken() and cookChicken() are associated with the same Monitor object.

### Check condition for wait() with while loop
wait() is usually used to check condition(e.g. hasChicken in the method eatChicken()). It is a good practice to always use wait() inside a while loop to check for the condition. This is to avoid the situation in which thread is woken for an unintended purpose. The example below illustrates this situation:
```
public synchronized void eatChicken() {
	if (!hasChicken) {
		wait();
	}
	eat();
	hasChicken = false;
}

public synchronized void drink() {
	if (!hasDrink) {
		wait();
	}
	drink();
	hasDrink = false;
}

public synchronized void cookChicken() {
	hasChicken = true;
	notifyAll();
}

public synchronized void prepareDrink() {
	hasDrink = true;
	notifyAll();
}
```

In this example, all four synchronized methods are associated with the same Monitor object. When both eatChicken() and drink() are called and are both put into the callback queue because current hasChicken is false and hasDrink is false, calling notifyAll() from cookChicken() will wake up both drink() and eatChicken() one after another. If while loop is not used to check for the condition, both drink() and eatChicken() will execute and the execution of drink() is not intended.
However, instead if while loop is used to check for the condition, when drink() is woken, hasDrink will be checked again and drink() will call wait() again. One alternative solution is to make sure all wait() and notifyAll() associated with the same Monitor object are checking the same conditions. However, this is hard to maintain and verify.

### Breakpoint of notifyAll()
One important point to take note when using notifyAll() is to understand what happens when notify all is called and how code gives up its Monitor object lock. Taking the following code snippet as an example:

```
public synchronized void foo() {
	before();
	notifyAll();
	after();
}
```

When synchronized method foo() is called, it calls notifyAll() before after(). However, after notifyAll() is called, the method foo() does not give up Monitor object lock immediately. Instead, all codes in the synchronized block(in this case the method body) will be executed before the thread gives up the Monitor object lock and notifies all the threads in the callback queue.

## notify()
Another feature of synchronized is notify(), which notifies only one thread in the callback queue. This method is less used than notifyAll() because notify() only wakes up one randomly chosen thread from callback queue and it is hard to assert that the thread woken up is the intended thread. However, there are special situations in which notify() is used and is preferred. When all threads that call wait() are checking for the same condition, it is no different to call anyone of the thread. notify() can be used in this situation and is better than nofityAll() because notifyAll() wake up all the threads and is, therefore, more costly. When one can be sure that all the threads in the callback queue are waiting for the same condition, one can use notify().

# Related areas
Java Synchronization as demonstrated in this tutorial is easy to learn, and you are ready to use it after this tutorial. However, Java Synchronization is just a part of a bigger picture: Parallel programs in Java. To fully exploits the parallelism brought by modern hardware, you may also want to learn [Java Thread][2] and [Thread Safety][5]


[1]: https://en.wikipedia.org/wiki/Monitor_(synchronization)
[2]: to be updated when @ablyx finishes on java threads
[3]: https://en.wikipedia.org/wiki/Mutual_exclusion
[4]: https://en.wikipedia.org/wiki/Critical_section
[5]: https://en.wikipedia.org/wiki/Thread_safety
[6]: https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html
[7]: https://en.wikipedia.org/wiki/Semaphore_(programming)

</div>
