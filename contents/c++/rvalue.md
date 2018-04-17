# Rvalue references and Move Semantics

Authors: [Tan Jun An](https://github.com/yamidark)

* [Background](#background)
    * [Value Semantics](#value-semantics)
    * [Reference Semantics](#reference-semantics)
* [Move Semantics](#move-semantics)
    * [rvalues and lvalues references](#rvalues-and-lvalues-references)
    * [Using rvalue references](#using-rvalue-references)
    * [Move Semantics](#move-semantics)
* [Rvalue anti-pattern](#rvalue-anti-pattern)
* [Resources](#resources)

## Background
*C++* is a general-purpose programming language designed to provide high performance and efficiency for resource-constrained and large systems. The language has been extended and improved after the past decades, with new standards being released periodically. One such standard, the [`C++11`](https://isocpp.org/wiki/faq/cpp11) standard, brought about many changes and improvements, that it made C++ feel like a different language altogether. One such feature that furthur improves on the performance on the language is the introduction of *rvalue* references and *Move Semantics*.

In order to better understand the benefits of using Move Semantics, it is important to first understand the two other modes already available in C++, *Value Semantics* and *References Semantics*.

### Value Semantics
Value (or copy) semantics is the programming style where we are only concerned about the values stored in the objects, rather than the object itself. As such, we will always create an extra copy of the value whenever we pass it to a function (also known as [pass-by-value](http://www.learncpp.com/cpp-tutorial/72-passing-arguments-by-value/)) or when constructing a new object and variable. This ensures that each object (or function) will have their own copy value to use, without having to concern themselves with their originator.

Some of the advantages of Value Semantics include:
    - No memory management issue, because there won't be any [dangling references](https://www.quora.com/What-is-dangling-reference) to objects that may not exist, nor any [memory leaks](https://www.geeksforgeeks.org/what-is-memory-leak-how-can-we-avoid/).
    - Ensures [referential transparency](https://en.wikipedia.org/wiki/Referential_transparency). Having our own copy of the value ensures changing the value in one object will not affect the original value. This is especially useful in a [multi-threaded](https://stackoverflow.com/questions/1313062/what-is-a-multithreaded-application) environment, as this prevents removes the need for synchronization of the values, allowing the program to run faster.
    - Speedup. If we require accessing of the value many times, having your own local copy of the value may be faster than having a pointer to the value and dereferencing it each time. This is especially true in C++ as it encourages Value Semantics, with optimization techniques such as [copy elision](http://en.cppreference.com/w/cpp/language/copy_elision) to help passing-by-value be faster.

However, Value Semantics has [one major flaw](https://www.quora.com/What-are-the-drawbacks-of-pass-by-value-result):
    - Poor performance and scalability. If we pass the value to a function that only reads the value in the object, creating an additional copy just for this purpose is an unnecessary consumption of memory and computation. This is especially true when we scale up to pass around objects or larger size, as more memory and time is needed to create this copy.

### Reference Semantics
Reference (or pointer) semantics is another choice available for users in C++. C++ allows users to declare [pointers](http://www.cplusplus.com/doc/tutorial/pointers/) and [references](https://www.geeksforgeeks.org/references-in-c/) that point to the address of the object. As such, we can pass around these pointers to functions, and all of them will refer to and use the same object and address.

Some advantages of Reference Semantics include:
    - Improve performance and scalability. By passing around pointers and references to the same object, we remove the need to create an additional copy of the object, thus overcoming the major flaw of Value Semantics.
    - Allows interaction between objects. with each other as they can have pointers to and modify the same value.

However, Reference Semantics also comes with its own problems, which include:
    - Indeterminant behaviour in a multi-threaded environment. Since each thread will all be pointing to the same object, this will lead to unnecessary data races and indeterminant behaviour when we modify these same objects in each thread. This in turn requires extra work in trying to synchronize the values between objects, which could also slow down the program. More details of this can be found in the [Java Concurrency](java/JavaConcurrency.md) and [Java Synchronization](java/JavaSynchronization.md).
    - Unintended behaviour. If we are not careful when passing around the pointers and references, we could unintentionally modify the same object  [article](http://www.drdobbs.com/cpp/optimization-calling-by-value-or-by-refe/232400151).

## Move Semantics
From the summary above, we know of many benefits we can get when using Value Semantics. However, it's one major flaw is that we will always create a copy of the value, which can computationally expensive if this value is a large object. However, we may also not want to use Reference Semantics due to its different problems as discussed above. As such, in order to continue gaining the benefits of Value Semantics while overcoming it's major flaw, `C++11` introduced a new mode to users, Move Semantics.

### rvalue and lvalue references
To understand how Move Semantics work in C++, it is important to distinguish between rvalue and lvalue references.
```
lvalue = rvalue
```
This line gives a basic understanding of what lvalue and rvalues are. lvalue (left value) basically refers to values that are addressable, whileas rvalue (right value) are temporary objects or values which are used only on the right side of an assignment expression. More details and classification of these 2 values can be found [here](http://www.bogotobogo.com/cplusplus/C11/4_C11_Rvalue_Lvalue.php).

### Using rvalue references
Rvalue references allows us to distinguish an lvalue from an rvalue. In C++11, we can declare an rvalue reference using the `&&` operator:
```cpp
int &&rvalue = 55;
```

We can also convert an lvalue to a rvalue using the `std::move` function:
```cpp
int lvalue = 99;
int &&rvalue2 = std::move(lvalue);
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

Using of these functions overload with rvalue parameters, which take on temporary objects, helps in writing more efficient program using Move Semantics!

### Move Semantics
The main usage of rvalue references is that it allows us to create *move* constructor and *move* assignments, instead of *copy* constructor and *copy* assignments by default. Since rvalues are typically temporary objects, we can just *move* the value instead of *copying* it, thus reducing on memory consumption and improving on the performance!

One example implementation of a move constructor and move assignment is shown below:
```cpp
Foo(Foo&& other) { // move constructor
    x = other.x;
    y = other.y;
    z = other.z;
}

Foo& operator=(Foo&& other) { // move assignment
    x = std::move(other.x);
    y = std::move(other.y);
    z = std::move(other.z);
    return *this;
}
```

In this move constructor and assignment, the contents of the `other` parameter is *moved* into the object, and the contents in `other` is destroyed afterwards. No additional memory allocation is required, and the *move* operation is done quickly by a few assignment of address operations, leading to a faster and more memory efficient program.

Along with Move Semantics in `C++11`, the [STL library](https://www.geeksforgeeks.org/the-c-standard-template-library-stl/) provides the overloaded *move* functions for its container classes (e.g. vector, list, set), thus we can take advantage of these Move Semantics by simply supplying rvalues, without the need to redefine the classes ourselves.

## rvalue anti-pattern
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

As shown above, the *move* constructor defines each of its parameter to be rvalues. In this case, if the constructor is called using a mixture of both lvalues and rvalues, such as lvalue for `x` and rvalue for `y`, the copy constructor instead will be called! This is because the *copy* constructor is able to accept both lvalues and rvalues, while the *move* constructor is only able to accept rvalues. As such, we may think we have made use of Move Semantics to optimize our program, but that may not be the case!

One solution to this problem would be to overload the constructor for each combination of rvalue parameters possible, like this:
```cpp
Foo(const std::string& x, const std::string& y) { // copy constructor
    _x = x;
    _y = y;
}

Foo(std::string&& x, const std::string& y) { // move constructor 1
    _x = x;
    _y = y;
}

Foo(const std::string& x, std::string&& y) { // move constructor 2
    _x = x;
    _y = y;
}

Foo(std::string&& x, std::string&& y) { // move constructor 3
    _x = x;
    _y = y;
}
```
However, this is not feasible, as we would require `2^n` overload functions, where `n` is the number of parameters. This results in large amount of [boilerplate code](https://www.quora.com/What-is-boilerplate-code), which in turn reduces code quality, and also contributes to increasd memory consumption and compilation time.

Rather, what we should be providing here is:
```cpp
Foo(std::string x, std::string y) { // move constructor
    _x = std::move(x);
    _y = std::move(y);
}
```

Yes, we revert back to the old Value Semantics type constructor instead. By doing so, we leave it to the caller to decide whether they want to have an additional copy by calling this constructor with `Foo(x,y)`, or to prevent the additional copy by calling `Foo(std::move(x), std::move(y))`, depending on which value we don't need a copy of.

### Resources
The following resources gives more readings on what was discussed, and a more in-depth tutorial on rvalue references and Move Semantics:

* [Biggest and important changes in C++11](https://blog.smartbear.com/development/the-biggest-changes-in-c11-and-why-you-should-care/)
* [Why Value Semantics is good to use](https://akrzemi1.wordpress.com/2012/02/03/value-semantics/)
* [Comparison on pass-by-value or pass-by-reference](http://www.informit.com/articles/article.aspx?p=2731935&seqNum=18)
* [Tutorial on what is a rvalue and what is a lvalue](http://www.bogotobogo.com/cplusplus/C11/4_C11_Rvalue_Lvalue.php)
* [Tutorial on using rvalue references and Move Semantics](http://www.bogotobogo.com/cplusplus/C11/5_C11_Move_Semantics_Rvalue_Reference.php)
* [Sample examples of using rvalue references Move Semantics](http://www.bogotobogo.com/cplusplus/C11/5B_C11_Move_Semantics_Rvalue_Reference.php)
* [The rvalue reference anti-pattern](http://cpptruths.blogspot.sg/2012/03/rvalue-references-in-constructor-when.html)
