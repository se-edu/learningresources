# Introduction to Performance Optimization

Author: [Ian Teo](https://github.com/IanTeo), [Phang Chun Rong](https://www.github.com/Crphang)

## Overview

Computer performance can be defined as the rate of work accomplished by a computer system. Even if execution time is not important for a particular application, it may be important to reduce CPU cycles so as to consume less power; from applications running in small battery operated devices to huge data centres.

> Premature optimization is the root of all evil.

I think many people have heard of this quote from Donald Knuth before. This quote is actually misinterpreted frequently, because of the lack of context. Here is the full quote:

> We should forget about small efficiencies, say about 97% of the time: premature optimization is the root of all evil. Yet we should not pass up our opportunities in that critical 3%.

You can find a nice explaination of the quote [here](http://wiki.c2.com/?PrematureOptimization).

This is not a guide on how to optimize that 3%, but rather, to explain standard optimization techniques that you should apply in all of your code, so that you do not create sub-optimal codes (premature pessimization). If you want to find out more about optimizing that 3%, you can find more at [Other Resources](#other-resources) below.

The 2 techniques you should know are:

* Using appropriate Data Structures and Algorithms
* Memory Management
* Using Cache Friendly codes

## Using Appropriate Data Structures and Algorithms

Using appropriate Data Structures and Algorithms can improve the execution speed of your program trememdously. However, it requires time to learn and understand the nuances of each Data Structure and Algorithm and when to use them.

A good example of how Data Structures and Algorithms can improve execution time is with [binary search](https://www.tutorialspoint.com/data_structures_algorithms/binary_search_algorithm.htm). When finding a particular element in a list, you need to search through the entire list. If it has a million entries, every find will require you to look through **1,000,000** entries. However, if you have a [sorted](https://www.tutorialspoint.com/data_structures_algorithms/sorting_algorithms.htm) list, you can use binary search to reduce the number of entries you have to look through to around **20**!

If you are interested in learning more about Data Structures and Algorithms, you can learn more from the following resources:

* [Geeksforgeeks.org](http://www.geeksforgeeks.org/data-structures/): Introduction to data structures with code examples
* [Introduction to Algorithms (Book)](https://www.amazon.com/Introduction-Algorithms-3rd-MIT-Press/dp/0262033844): A good read for beginners interested in data structures and algorithms
* [MIT online course](http://courses.csail.mit.edu/6.851/spring12/lectures/): A online course by MIT on data structures and algorithms
* [topcoder.com](https://www.topcoder.com/community/data-science/data-science-tutorials/the-importance-of-algorithms/): A good write up on the importance of algorithms

## Memory Management

Memory management is important for performance optimization for Computer systems. One of the common techniques in [algorithms optimisation](#using-appropriate-data-structures-and-algorithms) is space and time trade off, where we increase runtime memory usage to decrease overall runtime. While this theoretically optimizes your system runtime, it might overall slowdown the system due to [Thrashing](https://en.wikipedia.org/wiki/Thrashing_(computer_science)). Detecting if the performance slowdown is memory related can be done with appropriate [memory profiling](PerformanceProfiling.md).

If you have ascertain that your system has memory related performance concerns, there are various solutions that you can employ to resolve them.

- Using generators to reduce memory used. [Generators](https://en.wikipedia.org/wiki/Generator_(computer_programming)) are functions that generates a sequence of values. Instead of returning an explicit array upfront, a generator returns a value at each iteration. This can [greatly reduced memory usage](https://letstalkdata.com/2015/05/how-to-use-python-generators-to-save-memory/) for large arrays.
- Sometimes memory usage of your program remains high because the unnecessary variables are yet to be freed from memory. If you are using a garbage collected language like Java, consider [tuning](https://www.javacodegeeks.com/2017/11/minimize-java-memory-usage-right-garbage-collector.html) your garbage collector to suit your needs. If such options is not good enough, you can explicitly free memory even in garbage collected language. An example from Python is shown below.

```python
import gc
gc.collect()
```

- Using appropriate variable types can also offer memory usage improvement. For example, we should prefer to use primitive `int` over `Integer` to reduce the overhead introduce by the `Integer` Object wrapper. This [guide](http://java-performance.info/overview-of-memory-saving-techniques-java/) for Java also propose ways to overcome obstacles introduced by the usage of primitives such as restrictions of JDK collections that requires Object wrappers.

## Using Cache Friendly code

Before we can talk about this, we need to know what the computer memory is. Computer memory has different components, [registers](https://en.wikipedia.org/wiki/Processor_register), [L1/L2/L3 cache](https://www.cs.umd.edu/class/fall2001/cmsc411/proj01/cache/cache.html), [RAM](https://en.wikipedia.org/wiki/Random-access_memory), and [disk](https://en.wikipedia.org/wiki/Hard_disk_drive) in order of their speed.

> You can take a look at the extent of the difference of their access speeds [here](https://gist.github.com/jboner/2841832) <br>
> You can also check out this [infographic](http://imgur.com/8LIwV4C)

To understand this topic, you only need to know how the cache works. It is okay if you do not understand exactly how the other components, registers, RAM and disk work. The cache is simply a place to store memory so that it can be accessed quickly. The cache is usually split into a few layers, L1, L2 and L3 cache, where the L1 cache is the fastest and L3 is the slowest. The faster a cache is, the more expensive it is. This means that the L1 cache (a few kilobytes big) is smaller than the L2 cache (a few megabytes big) and so on, until the RAM (a few gigabytes big).

Whenever data is requested, the computer will first look in the cache for the data. If it exists, this is known as a **cache hit**. If it does not exist, this is known as a **cache miss**. When a **cache miss** happens, a contiguous block of memory containing the requested data is retrieved and stored onto the cache. Because of this, we want to remember these rules to make cache friendly codes:

* Temporal Locality
* Spatial Locality
* Row/Column Major Order

#### Temporal Locality

This rule states that recently used memory will likely be used in the near future. This means that making the scope of your variables smaller helps with execution times, as it will likely result in less cache misses.

#### Spatial Locality

This rule states that memory stored near each other will likely be used in the near future. This means that using contiguous data structures, such as arrays, help improve the execution times. This is because the contiguous block of memory will likely contain the other elements of the array, resulting in less **cache misses**.

An example of a Data Structure that does not do well in this aspect is [Linked List](https://www.tutorialspoint.com/data_structures_algorithms/linked_list_algorithms.htm). In a Linked List, each node can be stored anywhere on the memory. This means that there will likely be more cache misses when trying to iterate through a Linked List. This can cause Linked Lists to be much slower than what you would expect in theory.

#### Row/Column Major Order

This rule is about how multidimensional arrays are stored in memory. Different programming languages have different methods of storing multidimensional arrays. [More Information](https://en.wikipedia.org/wiki/Row-_and_column-major_order).

Using the incorrect method of access can cause many cache misses, resulting in a much slower execution time. Thus, it is important to be aware of which major order the programming language is using.

For example, Java uses Row Major Order. We can create a test to see how big an impact using the wrong Major Order can be on the execution time.

```python
int size = 10000;
int[][] arr = new int[size][size];
int x = 0;
 
//Row Major order accessing
long time = System.currentTimeMillis();
for (int i = 0; i < size; i++) {
    for (int j = 0; j < size; j++) {
        x += arr[i][j];
    }
}
System.out.println("Row major: " + (System.currentTimeMillis() - time));
 
//Column Major order accessing
time = System.currentTimeMillis();
for (int i = 0; i < size; i++) {
    for (int j = 0; j < size; j++) {
        x += arr[j][i];
    }
}
System.out.println("Column major: " +(System.currentTimeMillis() - time));
```

In the example above, Row major takes around 100ms, while column major takes around 2000ms. You can use the codes above and try it on your own too.

> This test was done on a typical notebook. Your results may vary based on the hardware of your computer

[Wikipedia](https://en.wikipedia.org/wiki/Locality_of_reference) and the [University of Maryland](https://www.cs.umd.edu/class/fall2001/cmsc411/proj01/cache/matrix.html) have excellent articles which covers everything I have mentioned and more.

## Other Resources

If you want to know more about Optimization, especially for that critical 3%, these other resources could be useful:

* Finding the critical path - [Performance Profiling](PerformanceProfiling.md)
