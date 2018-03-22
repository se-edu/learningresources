# Rust

Author(s): [Tan Li Hao](https://github.com/LiHaoTan)

## Why Rust
Rust is a relatively new programming language, so as such the benefits of learning or using the language is not very clear. It is also important to know the merits of a language so we know when to use them.

The main merits would be in its [design](https://www.rust-lang.org/en-US/faq.html#what-is-this-projects-goal):
* [safe](#safe) (e.g. memory safe)
* [concurrent](#concurrent)
* [practical](#practical)

### Safe

Designing the language to be safe allows common errors such as segmentation faults ([[1]](https://en.wikipedia.org/wiki/Segmentation_fault), [[2]](https://stackoverflow.com/questions/2346806/what-is-a-segmentation-fault/2346849#2346849)), [resource leaks](http://blog.skylight.io/rust-means-never-having-to-close-a-socket/), and [many others](https://www.reddit.com/r/rust/comments/2mwpie/what_are_the_advantages_of_rust_over_modern_c/) to be avoided. Many of these pitfalls can already be avoided in modern languages such as [RAII](https://en.wikipedia.org/wiki/Resource_acquisition_is_initialization) in C++ and [try-with-resources](https://blogs.oracle.com/darcy/more-concise-try-with-resources-statements-in-jdk-9) in Java, but mostly still requires developer discipline to ensure safety ([[1]](https://www.rust-lang.org/en-US/faq.html#why-rust-vs-cxx), [[2]](https://www.reddit.com/r/rust/comments/2mwpie/what_are_the_advantages_of_rust_over_modern_c/)).

The underlying concept for the safety provided in Rust is called [ownership](https://doc.rust-lang.org/book/second-edition/ch04-00-understanding-ownership.html) and is [translatable to other languages](https://codewithoutrules.com/2017/01/26/object-ownership/), and safety is ensured by Rust compile-time.

### Concurrent

Concurrency is getting [increasingly important](https://softwareengineering.stackexchange.com/questions/115474/why-should-i-know-concurrent-programming) but it is challenging to write concurrent code ([[1]](https://news.ycombinator.com/item?id=8138578), [[2]](https://golang.org/doc/faq#csp), [[3]](http://joeduffyblog.com/2016/11/30/15-years-of-concurrency/)).

Rust provides concurrency which is built upon the safety concepts. The implication is that the safety concepts allows us to be fearless when writing concurrent code by helping pointing out mistakes compile time [[1]](https://blog.rust-lang.org/2015/04/10/Fearless-Concurrency.html).

### Practical

Rust is designed to be practical, but this term is vague and does not say much. Instead, we can take a look at some points of Rust's guiding design:
* Uses old established techniques instead of particularly cutting-edge technologies
* Provides only majority-case features
* Runs on widely used platforms without unnecessary compromises

See [non-goals](https://www.rust-lang.org/en-US/faq.html#what-are-some-non-goals).

#### Production Ready
Also, in order for a language to be practical it must be usable in production. Aside from [Servo](https://github.com/servo/servo), the prototype browser engine Mozilla is working on, Rust is also used in companies such as Dropbox [[1]](https://news.ycombinator.com/item?id=11283688). 

#### Developer experience
Practicality can also be measured with how much developers enjoy using the language and want to continue using it, because if the language is not very practical the developer experience would not be very good. For Rust, it is a language that developers want to continue using ([[1]](https://www.reddit.com/r/rust/comments/842adc/rust_voted_most_loved_language_for_the_3rd_year/dvmftnk/), [[2]](https://medium.com/mozilla-tech/why-rust-is-the-most-loved-language-by-developers-666add782563)).

Although Rust does not use particularly cutting-edge technologies, Rust is still modern in that it is [significantly influenced by functional programming](https://doc.rust-lang.org/book/second-edition/ch13-00-functional-features.html), and has a type system that is [drawn from Haskell's typeclasses](https://www.rust-lang.org/en-US/faq.html#compare-go-and-rust). This allows Rust to also benefit from the advantages of functional programming. 

There are also a lot of other details that go into making a great developer experience, such as tooling and documentation. However, instead of going into more details, let's look at a resource that summarizes why others think Rust is great:

[fireflowers - The Rust Programming Language, in the words of its practitioners](https://brson.github.io/fireflowers/)

## How to learn Rust

Knowing the merits of a language without knowing how to learn it however is not sufficient. As mentioned, part of making the developer experience great is good documentation. And Rust has many great resources to learn it.

### Rust is difficult

There is this notion however that Rust is difficult that we need to address first.

It is true that there are many who say that Rust is difficult, and we should certainly acknowledge it. So while we cannot then pretend that the language is easy, there has however since been many [improvements](https://blog.rust-lang.org/2017/12/21/rust-in-2017.html#rust-should-have-a-lower-learning-curve) which reduces the learning curve.

### Resources to learn Rust

There are already great official resources to learn Rust, for instance:
- [The Rust Programming Language book](https://doc.rust-lang.org/book/)
- [Rust By Example](https://rustbyexample.com/)
- [Frequently Asked Questions](https://www.rust-lang.org/en-US/faq.html)

There is also a [large collection of community-maintained resources](https://github.com/ctjhoa/rust-learning), and the following are some resources that are possibly useful:
* [Comparison with Other Languages](https://github.com/ctjhoa/rust-learning#comparison-with-other-languages)
* [Cheat sheets](https://github.com/ctjhoa/rust-learning#cheat-sheets)
* [Ownership / Concurrency](https://github.com/ctjhoa/rust-learning#ownership--concurrency)

More resources: https://www.rust-lang.org/en-US/documentation.html

# Why not Rust

Choosing the right language for the job is important so let's look at a few possible reasons not to use Rust:
- [The ecosystem is not as mature](https://github.com/ctjhoa/rust-learning#are-we--yet)
- Rust's compiler is extremely strict in that it enforces ownership, i.e. needs time getting used to. However, it is [probably debatable if this approach is a worthwhile trade-off](https://news.ycombinator.com/item?id=16202373).
- While not completely similar, [Go feels immediately more productive](https://news.ycombinator.com/item?id=13430108)

Some other discussions on why not Rust:
- https://www.reddit.com/r/rust/comments/7p75ab/why_rust_what_i_want_changed_for_rust_to_help_way/
- https://www.reddit.com/r/rust/comments/6hp54n/blog_why_not_to_use_rust/

