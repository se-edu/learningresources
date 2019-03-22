<frontmatter>
  title: Rvalue References and Move Semantics
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Rvalue References and Move Semantics

Authors: [Tan Jun An](https://github.com/yamidark)

* [Background](#background)
  * [Value Semantics](#value-semantics)
  * [Reference Semantics](#reference-semantics)
* [Move Semantics](#move-semantics)
  * [Rvalue and Lvalue References](#rvalue-and-lvalue-references)
  * [Using Rvalue References](#using-rvalue-references)
  * [Move Semantics](#move-semantics)
* [Rvalue Anti-Pattern](#rvalue-anti-pattern)
* [Resources](#resources)

## Background
*C++* is a general-purpose programming language designed to provide high performance and efficiency for resource-constrained and large systems. The language has since been extended and improved upon, with new standards being released periodically. One such standard, the `C++11` standard, improved the performance of the language with features such as **_Rvalue References_** and **_Move Semantics_**.

In order to better understand the benefits of using Move Semantics, it is important to first understand the two other modes already available in C++, **_Value Semantics_** and **_References Semantics_**.

### Value Semantics
Value (or copy) semantics is the programming style where users are only concerned about the values stored in the objects, rather than the object itself. As such, an extra copy of the object will always be created whenever it is passed to a function, (also known as [pass-by-value](http://www.learncpp.com/cpp-tutorial/72-passing-arguments-by-value/)) or when constructing or assigning a new object. This ensures that each object declared (or function) will have their own copied value to use, without having to concern themselves with their originator. By default, C++ uses this mode if variables are declared with only the data type.

Some advantages of Value Semantics include:
* Lesser memory management issues, as there won't be any [dangling references](https://www.quora.com/What-is-dangling-reference) to objects that may not exist, nor any [memory leaks](https://www.geeksforgeeks.org/what-is-memory-leak-how-can-we-avoid/).
* [Referential transparency](https://en.wikipedia.org/wiki/Referential_transparency). Having our own copy of the object ensures that changing the values inside the copied object will not affect the original object. This is especially useful in a [multi-threaded](https://stackoverflow.com/questions/1313062/what-is-a-multithreaded-application) environment, as it removes the need to synchronise the object's values, allowing the program to run faster.
* Better performance. If the function requires accessing of the object's value many times, having a local copy of the object may be faster than having a pointer to the object and dereferencing it each time. This is especially true in C++ which encourages Value Semantics, with optimization techniques such as [copy elision](http://en.cppreference.com/w/cpp/language/copy_elision) in the compiler which speeds up the process of passing-by-value.

However, Value Semantics has [one major flaw](https://www.quora.com/What-are-the-drawbacks-of-pass-by-value-result):
* Poor performance and scalability. If the function called is a read-only function, creating an additional copy will be an unnecessary consumption of memory and runtime. This is especially true when scaling up to pass around objects of larger size, as more memory and runtime is needed to create each copy.

### Reference Semantics
Reference (or pointer) semantics is another choice available for use in C++, which allows users to declare [pointers](http://www.cplusplus.com/doc/tutorial/pointers/) and [references](https://www.geeksforgeeks.org/references-in-c/) that point to the address of the object. As such, we can pass these reference address to functions and declaration of other pointers, and all of them will refer to and use the same object and address.

Some advantages of Reference Semantics include:
* Improved performance and scalability. By passing around pointers and references to the same object, functions can just use the values in that object directly without having to create an extra copy, thus overcoming the major flaw of Value Semantics.
* Interactions between objects and functions. Since the same object can be referred to by different pointers, its value can be modified in different areas of the program.

However, Reference Semantics also comes with its own problems, such as:
* Indeterminant behaviour. In a multi-threaded environment, since each thread will all be pointing to the same object, this will lead to unnecessary [data races](https://docs.oracle.com/cd/E19205-01/820-0619/geojs/index.html) and indeterminant behaviour as they are modified concurrently in each thread. This in turn requires extra work to synchronize the values between objects, which also slows down the program. More details of this can be found in the resources [Java Concurrency](../java/JavaConcurrency.html) and [Java Synchronization](../java/JavaSynchronization.html).
* Harder to debug. If users are not careful, they may modify the values in the object using one pointer, which will then be reflected by another pointer to the same object in a different part of the program, which can confuse users as they may not know where and why the values are changing. A good example of this problem can be found in this [article](http://www.drdobbs.com/cpp/optimization-calling-by-value-or-by-refe/232400151).

## Move Semantics
From the summary above, there are many benefits for using Value Semantics. However, its one major flaw is that a copy of the object will always be created, which can be computationally expensive if this object is of a large size. Reference Semantics may also not be preferred due to its different problems as discussed above. As such, in order to continue gaining the benefits Value Semantics have over Reference Semantics while overcoming it's major flaw, `C++11` introduced a new mode to users, Move Semantics.

### Rvalue and Lvalue References
To understand how Move Semantics work in C++, it is important to distinguish between an rvalue and an lvalue.
```
lvalue = rvalue
```
From the line above, lvalue (left value) basically refers to values that are addressable, while rvalue (right value) are temporary objects or values which are used only on the right side of an assignment expression. More details and classification of these 2 values can be found [here](http://www.bogotobogo.com/cplusplus/C11/4_C11_Rvalue_Lvalue.php).

### Using Rvalue References
Rvalue References allows us to distinguish an lvalue from an rvalue. In `C++11`, we can declare an Rvalue Reference using the `&&` operator:
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

void print(int&& rvalue) { // takes a rvalue
    std::cout << "rvalue method used";
}

int main() {
    int x = 5;
    print(x); // lvalue method used
    print(10); // rvalue method used.
}
```

Usage of function overloading with Rvalue Reference parameters, which take on temporary objects, helps in writing more efficient programs using Move Semantics!

### Move Semantics
The main usage of Rvalue References is that it allows us to create *move* constructor and *move* assignments, instead of *copy* constructor and *copy* assignments by default. Since rvalues are typically temporary objects, we can just *move* the value instead of *copying* it, thus reducing  memory consumption and improving performance!

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

Along with Move Semantics in `C++11`, the [STL library](https://www.geeksforgeeks.org/the-c-standard-template-library-stl/) provides the overloaded *move* functions for its container classes (e.g. `vector`, `list`, `set`), thus we can take advantage of these Move Semantics by simply supplying rvalues, without the need to redefine the classes ourselves.

## Rvalue Reference Anti-Pattern
After learning about the new Rvalue References and Move Semantics in `C++11`, many programmers from the older C++ eras may fall into a trap of the Rvalue Reference [Anti-Pattern](https://en.wikipedia.org/wiki/Anti-pattern).

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

As shown above, the *move* constructor defines each of its parameter to be Rvalue References. In this case, if the constructor is called using a mixture of both lvalues and rvalues, such as lvalue for `x` and rvalue for `y`, the copy constructor instead will be called! This is because the *copy* constructor is able to accept both lvalues and rvalues, while the *move* constructor is only able to accept rvalues. As such, we may think we have made use of Move Semantics to optimize our program, but that may not be the case!

One solution to this problem would be to overload the constructor for each combination of Rvalue Reference parameters possible, like this:
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
However, this is not feasible as we would require `2^n` overload functions, where `n` is the number of parameters. This results increased [boilerplate code](https://www.quora.com/What-is-boilerplate-code), which in turn reduces code quality, increases memory consumption and compilation time.

Rather, what should be provided here is:
```cpp
Foo(std::string x, std::string y) { // move constructor
    _x = std::move(x);
    _y = std::move(y);
}
```

Yes, revert back to using the old Value Semantics type constructor instead! By doing so, it is now up to the caller to decide whether they want to have an additional copy by calling this constructor with `Foo(x,y)`, or to prevent the additional copy by calling `Foo(std::move(x), std::move(y))`, depending on which value is no longer needed.

## Resources
The following resources gives more readings on what was discussed, and a more in-depth tutorial on Rvalue References and Move Semantics:

* [Biggest and important changes in C++11](https://blog.smartbear.com/development/the-biggest-changes-in-c11-and-why-you-should-care/)
* [Why Value Semantics is good to use](https://akrzemi1.wordpress.com/2012/02/03/value-semantics/)
* [Comparison on pass-by-value or pass-by-reference](http://www.informit.com/articles/article.aspx?p=2731935&seqNum=18)
* [Tutorial on what is a rvalue and what is a lvalue](http://www.bogotobogo.com/cplusplus/C11/4_C11_Rvalue_Lvalue.php)
* [Tutorial on using Rvalue References and Move Semantics](http://www.bogotobogo.com/cplusplus/C11/5_C11_Move_Semantics_Rvalue_Reference.php)
* [C++ Rvalue References Explained in 11 Sections](http://thbecker.net/articles/rvalue_references/section_01.html)
* [Sample examples of using Rvalue References and Move Semantics](http://www.bogotobogo.com/cplusplus/C11/5B_C11_Move_Semantics_Rvalue_Reference.php)
* [The Rvalue Reference Anti-Pattern](http://cpptruths.blogspot.sg/2012/03/rvalue-references-in-constructor-when.html)

</div>
