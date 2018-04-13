# RValue references and Move Semantics

Authors: [Tan Jun An](https://github.com/yamidark)

* Background
* Pre-C++11
    * Value Semantics
    * Reference Semantics
* Move Semantics
    * RValues and LValues
    * Using rvalue references

## Background
C++ is a general-purpose programming language that was designed for resource-constrained and large systems, high performance and efficiency. The language has been extended and improved after the past decades, with new standards being released periodically every few years. One such standard, the C++11 standard, brought about many changes and improvements, that it made C++ feel like a different language altogether. One such feature that furthur improves on the performance on the language is the introduction of *rvalue* references and *Move Semantics*.

## Pre-C++11
This section gives an overview of the 2 modes available prior to C++11 standard, and also the advantages and disadvantages of each.

### Value Semantics
Value (or copy) semantics is given to users by default when we declare arugments with only the type name (e.g. `int`, `string`). It is the programming style where we are only concerned about the values stored in the objects, rather than the object itself. As such, we will always create an extra copy of the value whenever we pass it to a function (also known as *pass by value*) or when constructing a new object and variable. This ensures that each object (or function) will have their own copy value to use, without having to concern themselves with their originator.

Some of the advantages of using Value Semantics is that it ensures that we no memory management issue, as we won't have any dangling references to objects that may not exist and no memory leaks.
Value Semantics also ensures referential transparency, since having our own copy of the value ensures changing the value in 1 object and function will not affect the original value. This is especially useful in a multi-threaded environment, as this prevents removes the need for synchronization of the values, allowing the program to run faster.
One odd and minor benefit of Value Semantics is that it may also speedup programs. If we require accessing the value many times, having your own local copy of the value may be faster than having a pointer to the value and dereferencing it each time. This is especially true in C++, which is

However, 1 major flaw of Value Semantics is having poor performance and scability. When we wish to only read the values without modifying it, creating an additional copy just for this purpose is an unnecessary computation, especially if we scale up to larger objects.

### Reference Semantics
Reference (or pointer) semantics is another mode available for users in C++. C++ allows users to declare pointers and references to the address of the value. As such, we can pass around these pointers to functions and objects, and all of them will refer to and use the same value (address). This allows functions and other objects to modify the values in the address, thus allowing them to interact with each other. Below is a short code snippet demonstrating declaring pointers, getting addresses and dereferencing of pointers.

```cpp
    int x = 10;
    int *y = &x // pointer declare with `*`, to the address of x using `&`

    *y += 2; // dereference the value pointed by y
    std::cout << "x is " << x << std::endl; // value of x is 12
    std::cout << "y is " << (*y) << std::endl; // value pointed by y is also 12
```

Some of the advantages of using Reference Semantics is that it allows us pass around large objects without having to create an additional copy, thus improving on the performance and scability of the program.



