# RValue references and Move Semantics

Authors: [Tan Jun An](https://github.com/yamidark)

* Background
* Value Semantics
* Reference Semantics
* Move Semantics
    * Rvalues and Lvalues references
    * Using rvalue references
    * Move Semantics
* Rvalue anti-pattern 

## Background
C++ is a general-purpose programming language that was designed for resource-constrained and large systems, high performance and efficiency. The language has been extended and improved after the past decades, with new standards being released periodically every few years. One such standard, the [C++11](https://isocpp.org/wiki/faq/cpp11) standard, brought about many changes and improvements, that it made C++ feel like a different language altogether. One such feature that furthur improves on the performance on the language is the introduction of *rvalue* references and *Move Semantics*.

Before we move on to talk about Move Semantics and rvalue references, we will first briefly go through the 2 other modes available in C++, [Value Semantics](https://akrzemi1.wordpress.com/2012/02/03/value-semantics/)) and References Semantics.

### Value Semantics
Value (or copy) semantics is given to users by default when we declare arugments with only the type name (e.g. `int`, `string`). It is the programming style where we are only concerned about the values stored in the objects, rather than the object itself. As such, we will always create an extra copy of the value whenever we pass it to a function (also known as [pass-by-value](http://www.learncpp.com/cpp-tutorial/72-passing-arguments-by-value/)) or when constructing a new object and variable. This ensures that each object (or function) will have their own copy value to use, without having to concern themselves with their originator.

Some of the advantages of using Value Semantics is that it ensures that we no memory management issue, as we won't have any [dangling references](https://www.quora.com/What-is-dangling-reference) to objects that may not exist and no [memory leaks](https://www.geeksforgeeks.org/what-is-memory-leak-how-can-we-avoid/).
Value Semantics also ensures [referential transparency](https://en.wikipedia.org/wiki/Referential_transparency), since having our own copy of the value ensures changing the value in 1 object and function will not affect the original value. This is especially useful in a [multi-threaded](https://stackoverflow.com/questions/1313062/what-is-a-multithreaded-application) environment, as this prevents removes the need for synchronization of the values, allowing the program to run faster.
One seemingly odd benefit of Value Semantics is that it may also speedup programs. If we require accessing the value many times, having your own local copy of the value may be faster than having a pointer to the value and dereferencing it each time. This is especially true in C++ as it encourages Value Semantics, with optimization techniques such as [copy elision](http://en.cppreference.com/w/cpp/language/copy_elision) to help passing by value be faster.

However, [1 major flaw](https://www.quora.com/What-are-the-drawbacks-of-pass-by-value-result) of Value Semantics and passing-by-value is having poor performance and scability. When we wish to only read the values without modifying it, creating an additional copy just for this purpose is an unnecessary computation, especially if we scale up to larger objects.

### Reference Semantics
Reference (or pointer) semantics is another mode available for users in C++. C++ allows users to declare [pointers](http://www.cplusplus.com/doc/tutorial/pointers/) and [references](https://www.geeksforgeeks.org/references-in-c/) to the address of the value. As such, we can pass around these pointers to functions and objects, and all of them will refer to and use the same value (address).

One large advantage of using Reference Semantics is that it allows us pass around large objects without having to create an additional copy, thus improving on the performance and scability of the program. It also allows objects to interact with each other as they can have pointers to and modify the same value.

However, Reference Semantics also comes with its own problems. In a multi-threaded environment, having such object pointing to the same value will lead to unnecessary data races and un-deterministic behaviour, and requires extra work in trying to synchronize the values between objects. More details of this can be found in the [Java Concurrency](java/JavaConcurrency.md) and [Java Synchronization](java/JavaSynchronization.md).
Reference Semantics and also lead to unintended behaviour if we modify the values in 1 object, which in turn affects the other objects which have pointers to the same values as well, an example shown in this [article](http://www.drdobbs.com/cpp/optimization-calling-by-value-or-by-refe/232400151).

## Move Semantics
As mentioned above, 1 major flaw of Value Semantics is that we will create a copy of the value, which can computationally expensive if this value is a large object. However, we may also not want to use Reference Semantics due to its different problems as discussed above. As such, in order to continue using Value Semantics without creating this unnecessary copy, C++ introduced a new mode, Move Semantics.

### Rvalue and Lvalue references
To understand how Move Semantics work in C++, we first need to know about the distinction between rvalue and lvalue references. 1 simple line to describe the differencce between these 2 is:
```
lvalue = rvalue
```
where lvalue (left value) is on the left side of the equation, and rvalue (right value) is on the right.
lvalue basically refers to values that are addressable, whileas rvalue are temporary objects or values which are used only on the right side of an assignment expression. More details and classification of these 2 values can be found [here](http://www.bogotobogo.com/cplusplus/C11/4_C11_Rvalue_Lvalue.php).

### Using rvalue references
Rvalue references allows us to distinguish an lvalue from an rvalue. In C++11, we can declare an rvalue reference using the `&&` operator:
```cpp
int &&rvalue = 55;
```

We can also convert an lvalue to a rvalue using the `std::move` function:
```cpp
int lvalue = 99;
int &&rvalue2 = std::move(lvalue); // std::move converts an lvalue to a rvalue
```

We can also do [function overloading](https://en.wikibooks.org/wiki/Computer_Programming/Function_overloading)
in order to determine whether the parameters given are lvalues or rvalues:

```cpp
void print(int& lvalue) { // takes an lvalue
    std::cout << "lvalue method used";
}

void print(int&& rvalue) { // take a rvalue
    std::cout << "rvalue method used";
}

int main() {
    int x = 5;
    print(x); // lvalue method used
    print(10); // rvalue method used.
}
```

Using of these functions overload with rvalue parameters, which take on temporary objects, helps us to write more efficient program using Move Semantics!

### Move Semantics
The main usage of rvalue references is that it allows us to create *move* constructor and *move* assignments, instead of *copy* constructor and *copy* assignments by default. Since rvalues are typically temporary objects, we can just *move* the value instead of *copying* it, thus helping us to save on memory consumption and improving on the performance on our program as well!

One example implementation of a move constructor and move assignment is:
```cpp
Foo(Foo&& other) { // move constructor
    x = other.x;
    y = other.y;
    z = other.z;
}

Foo& operator=(Foo&& other) { // move assignment
    x = other.x;
    y = other.y;
    z = other.z;
    return *this;
}
```

In this move constructor and assignment, the contents of the `other` parameter is *moved* into the object, and the contents in `other` is destroyed afterwards. No additional memory allocation occurs, and the *move* operation is done quickly by a few assignment of address operations, leading to a more performance and memory efficient program.

Along with Move Semantics in C++11, the [STL library](https://www.geeksforgeeks.org/the-c-standard-template-library-stl/) functions also defines the overloaded *move* functions for its container classes, thus we can take advantage of these Move Semantics by simply supplying rvalues, without the need to redefine the classes ourselves.

## Rvalue anti-pattern
After learning about the new rvalue references and Move Semantics in C++11, many programmers from the older C++ eras may fall into a trap of the rvalue [anti-pattern](https://en.wikipedia.org/wiki/Anti-pattern).

Consider these 2 class constructors:
```cpp
Foo(const std::string& x, const std::string& y) { // copy constructor
    _x = x;
    _y = y;
}

Foo(std::string&& x, std::string&& y) { // move constructor
    _x = x;
    _y = y;
}
```

As we can see, the copy constructor defines each of its parameter to be rvalues. In this case, if we were to call the constructor using a mixture of both, lvalue for `x` and rvalue for `y`, the copy constructor instead will be called! This is because the copy constructor and take both lvalues and rvalues, both the move constructor can only take rvalues. As such, we may think we have made use of Move Semantics to optimize our program, but that may not be the case!

1 solution to this problem would be to overload the constructor for each combination of rvalue parameters possible. However, this is not feasible, especially when the function have many parameters, as we would require 2^n overload functions where n is the number of parameters, leading to too much [boilerplate code](https://www.quora.com/What-is-boilerplate-code), reducing code quality, and increasing memory consumption and compiliation time.

Rather, what we should be proving here is:
```cpp
Foo(std::string x, std::string y) { // move constructor
    _x = std::move(x);
    _y = std::move(y);
}
```

Yes, we revert back to the old Value Semantics type constructor instead. By doing so, we leave it to the caller to decide whether they want to have an additional copy by calling this constructor with `Foo(x,y)`, or to prevent the additional copy by calling `Foo(std::move(x), std::move(y))`, depending on which value we don't need a copy of.



