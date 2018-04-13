# RValue references and Move Semantics

Authors: [Tan Jun An](https://github.com/yamidark)

* Background
* Pre-C++11
    * Value Semantics
    * Reference Semantics
* Move Semantics
    * RValues and LValues references
    * Using rvalue references

## Background
C++ is a general-purpose programming language that was designed for resource-constrained and large systems, high performance and efficiency. The language has been extended and improved after the past decades, with new standards being released periodically every few years. One such standard, the [C++11](https://isocpp.org/wiki/faq/cpp11) standard, brought about many changes and improvements, that it made C++ feel like a different language altogether. One such feature that furthur improves on the performance on the language is the introduction of *rvalue* references and *Move Semantics*.

## Pre-C++11
This section gives an overview of the 2 modes available prior to C++11 standard, and also the advantages and problems caused by each.

### Value Semantics
[Value (or copy) semantics](https://akrzemi1.wordpress.com/2012/02/03/value-semantics/) is given to users by default when we declare arugments with only the type name (e.g. `int`, `string`). It is the programming style where we are only concerned about the values stored in the objects, rather than the object itself. As such, we will always create an extra copy of the value whenever we pass it to a function (also known as *pass by value*) or when constructing a new object and variable. This ensures that each object (or function) will have their own copy value to use, without having to concern themselves with their originator.

Some of the advantages of using Value Semantics is that it ensures that we no memory management issue, as we won't have any dangling references to objects that may not exist and no memory leaks.
Value Semantics also ensures [referential transparency](https://en.wikipedia.org/wiki/Referential_transparency), since having our own copy of the value ensures changing the value in 1 object and function will not affect the original value. This is especially useful in a multi-threaded environment, as this prevents removes the need for synchronization of the values, allowing the program to run faster.
One seemingly odd benefit of Value Semantics is that it may also speedup programs. If we require accessing the value many times, having your own local copy of the value may be faster than having a pointer to the value and dereferencing it each time. This is especially true in C++ as it encourages Value Semantics, with optimization techniques such as [copy elision](http://en.cppreference.com/w/cpp/language/copy_elision) to help passing by value be faster.

However, 1 major flaw of Value Semantics is having poor performance and scability. When we wish to only read the values without modifying it, creating an additional copy just for this purpose is an unnecessary computation, especially if we scale up to larger objects.

### Reference Semantics
[Reference (or pointer) semantics]() is another mode available for users in C++. C++ allows users to declare pointers and references to the address of the value. As such, we can pass around these pointers to functions and objects, and all of them will refer to and use the same value (address).

One large advantage of using Reference Semantics is that it allows us pass around large objects without having to create an additional copy, thus improving on the performance and scability of the program. It also allows objects to interact with each other as they can have pointers to and modify the same value.

However, Reference Semantics also comes with its own problems. In a multi-threaded environment, having such object pointing to the same value will lead to unnecessary data races and un-deterministic behaviour, and requires extra work in trying to synchronize the values between objects. Reference Semantics and also lead to unintended behaviour if we modify the values in 1 object, which in turn affects the other objects which have pointers to the same values as well, an example shown in this [article](http://www.drdobbs.com/cpp/optimization-calling-by-value-or-by-refe/232400151).

## Move Semantics
As mentioned above, 1 major flaw of Value Semantics is that we will create a copy of the value, which can computationally expensive if this value is a large object. However, we may also not want to use Reference Semantics due to its different problems as disuccsed above. As such, in order to avoid this unnecessary copy, C++ introduced a new mode, Move Semantics.

### RValue and LValue references
To understand how Move Semantics work in C++, we first need to know about the distinction between rvalue and lvalue references. 1 simple line to describe the differencce between these 2 is:
```
lvalue = rvalue
```
where lvalue (left value) is on the left side of the equation, and rvalue (right value) is on the right.
lvalue basically refers to values that are addressable, whileas rvalue are temporary objects or values which are used only on the right side of an assignment expression. More details and classification of these 2 values can be found [here](http://www.bogotobogo.com/cplusplus/C11/4_C11_Rvalue_Lvalue.php).

### Using rvalue references

