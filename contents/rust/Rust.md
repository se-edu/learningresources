# Rust

Author(s): [Tan Li Hao](https://github.com/LiHaoTan)

## Why Rust
Rust is a multi-paradigm (e.g. functional and imperative) systems language, but as it is relatively new the benefits of learning or using the language is not very clear. In any case, it is important to know the merits of a language so we know when to use them.

The main merits would be in its [design](https://www.rust-lang.org/en-US/faq.html#what-is-this-projects-goal):
* [Safety](#safety) (e.g. memory safe)
* [Better support for concurrency](#better-support-for-concurrency)
* [Practicality](#practicality)

### Safety

Designing the language to be safe allows common safety errors such as segmentation faults ([[1]](https://en.wikipedia.org/wiki/Segmentation_fault), [[2]](https://stackoverflow.com/questions/2346806/what-is-a-segmentation-fault/2346849#2346849)), [resource leaks](http://blog.skylight.io/rust-means-never-having-to-close-a-socket/), and [many others](https://www.reddit.com/r/rust/comments/2mwpie/what_are_the_advantages_of_rust_over_modern_c/) to be avoided. Many of these pitfalls can already be avoided in modern languages such as [RAII](https://en.wikipedia.org/wiki/Resource_acquisition_is_initialization) in C++ and [try-with-resources](https://blogs.oracle.com/darcy/more-concise-try-with-resources-statements-in-jdk-9) in Java, but mostly still requires developer discipline to ensure safety ([[1]](https://www.rust-lang.org/en-US/faq.html#why-rust-vs-cxx), [[2]](https://www.reddit.com/r/rust/comments/2mwpie/what_are_the_advantages_of_rust_over_modern_c/)).

The underlying concept for the safety provided in Rust is called [ownership](https://doc.rust-lang.org/book/second-edition/ch04-00-understanding-ownership.html). Intuitively, the concept just means that each value only has a single owner (i.e. a variable) and if the value needs to be shared, it can be borrowed. This is a powerful concept that ensures safety in Rust compile-time and [can be applied to other languages](https://codewithoutrules.com/2017/01/26/object-ownership/).


Take a simplified example in Java of a class called `VisibleIndexes` maintaining the list of visible indexes of UI elements:
```Java
class VisibleIndexes {
    private List<Integer> indexes;

    public VisibleIndexes() {
        indexes = new ArrayList<>();
    }

    public void addIndex(int index) {
        indexes.add(index);
    }

    public List<Integer> getVisibleIndexes() {
        return Collections.unmodifiableList(indexes);
        
        // Should not do this because indexes can then be modified outside class
        //return indexes; 
    }
}
```

One would need the discipline to make sure to return only a view of the list, otherwise the following would be legal, but unexpected:

```Java
VisibleIndexes visibleIndexes = new VisibleIndexes();
visibleIndexes.addIndex(1);
List<Integer> indexes= visibleIndexes.getVisibleIndexes();
System.out.println(indexes); // Output: [1]

listCounts.add(9999); // Unexpected mutation of the internal list in VisibleIndexes
System.out.println(visibleIndexes.getVisibleIndexes()); // Output: [1, 9999]
```

In Rust, we can implement it like this:
```Rust
struct VisibleIndexes {
    indexes: Vec<i32>
}

impl VisibleIndexes {
    pub fn new() -> VisibleIndexes {
        VisibleIndexes {
            indexes: Vec::new(),
        }
    }

    pub fn add_index(&mut self, value: i32) {
        self.indexes.push(value);
    }

    pub fn get_visible_indexes(&mut self) -> &mut Vec<i32> {
        &mut self.indexes
    }
}
```
In particular, we should pay attention to `&mut Vec<i32>` which means to return a mutable reference to a Vector (List in Java).

Due to Rust's defaults, variables are by default immutable, and `self.indexes` can only have a single owner which will be the instance of `VisibleIndexes` instantiated.

One way to allow for mutation outside the instance would be to borrow the value (by using a reference) and it would have to be a mutable one for us to be able to modify the internal list `indexes` outside an instance of `VisibleIndexes`.

So we have to go through some hoops just to be able to modify the internal list but it doesn't end here. Suppose we then execute the following:

```Rust
let mut visible_indexes = VisibleIndexes::new();
visible_indexes.add_index(1);

let indexes = visible_indexes.get_visible_indexes();
println!("{:?}", indexes); // Output: [1]

indexes.push(9999);
println!("{:?}", visible_indexes.get_visible_indexes()); // Cannot compile!
```

Everything would have executed like in Java except it does not compile because of the last line in Rust. This is shown in the error message below: 
```shell
let indexes = visible_indexes.get_visible_indexes()
              --------------- first mutable borrow occurs here
...
println!("{:?}", visible_indexes.get_visible_indexes()); // Cannot compile!
                 ^^^^^^^^^^^^^^^ second mutable borrow occurs here
```

In any single scope, there can only be one mutable borrow. However, the scope of the value borrowed by `indexes` does not end until the end of a block (i.e. a closing brace). We then attempt to borrow the same value again in the same scope which will not compile in Rust.

### Better support for concurrency

Concurrency is getting [increasingly important](https://softwareengineering.stackexchange.com/questions/115474/why-should-i-know-concurrent-programming) but it is challenging to write concurrent code ([[1]](https://news.ycombinator.com/item?id=8138578), [[2]](https://golang.org/doc/faq#csp), [[3]](http://joeduffyblog.com/2016/11/30/15-years-of-concurrency/)).

Rust provides concurrency which is built upon the safety concepts. The implication is that the safety concepts allows us to be [fearless when writing concurrent code](https://blog.rust-lang.org/2015/04/10/Fearless-Concurrency.html) by helping point out mistakes compile time. 

We look at one [example](https://doc.rust-lang.org/book/second-edition/ch16-02-message-passing.html) of the increasingly popular approach to concurrency called message passing:
```Rust
fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        let val = String::from("hi");
        tx.send(val).unwrap();
        println!("val is {}", val); // this line does not compile!
    });

    let received = rx.recv().unwrap();
    println!("Got: {}", received);
}
```

In the example, the message `"hi"` from the new thread is passed to the main thread. However, it does not compile and produce the following error message:
```Shell
tx.send(val).unwrap();
        --- value moved here
println!("val is {}", val);
                      ^^^ value used here after move
```
When sending a message in Rust, the ownership is transferred (moved). Hence, using the value after the ownership is transferred would not compile. This is important because the string `"hi"` can be mutated by the receiving thread before `println!` executes, yielding unexpected behavior.

### Practicality

Rust is designed to be practical, as shown in Rust's guiding design:
* Uses old established techniques instead of particularly cutting-edge technologies
* Provides only majority-case features
* Runs on widely used platforms without unnecessary compromises

See [non-goals](https://www.rust-lang.org/en-US/faq.html#what-are-some-non-goals).

#### Production Ready
Also, in order for a language to be practical it must be usable in production. A well known large project is [Servo](https://github.com/servo/servo), the prototype browser engine Mozilla is working on. On top of that, there are many other [organizations running Rust in production](https://www.rust-lang.org/en-US/friends.html). As an example, Jamie Turner from Dropbox [explains the reasons for using Rust](https://news.ycombinator.com/item?id=11283688).

#### Developer experience
Practicality can also be measured with how much developers enjoy using the language and want to continue using it, because if the language is not very practical the developer experience would not be very good. For Rust, it is a language that developers want to continue using ([[1]](https://www.reddit.com/r/rust/comments/842adc/rust_voted_most_loved_language_for_the_3rd_year/dvmftnk/), [[2]](https://medium.com/mozilla-tech/why-rust-is-the-most-loved-language-by-developers-666add782563)).

Although Rust does not use particularly cutting-edge technologies, Rust is still modern in that it is [significantly influenced by functional programming](https://doc.rust-lang.org/book/second-edition/ch13-00-functional-features.html), and has a type system that is [drawn from Haskell's typeclasses](https://www.rust-lang.org/en-US/faq.html#compare-go-and-rust). This allows Rust to also benefit from the advantages of functional programming. 

There are also a lot of other details that go into making a great developer experience, such as tooling and documentation. However, instead of going into more details, let's look at a resource that summarizes why others think Rust is great:

[fireflowers - The Rust Programming Language, in the words of its practitioners](https://brson.github.io/fireflowers/)

## How to learn Rust

Knowing the merits of a language without knowing how to learn it however is not sufficient. As [mentioned](#developer-experience), part of making the developer experience great is good documentation. And there are many great avenues to learn Rust.

### Rust is difficult

Many have said Rust is difficult but we should not be so concerned because there has since been many [improvements](https://blog.rust-lang.org/2017/12/21/rust-in-2017.html#rust-should-have-a-lower-learning-curve) to reduce the learning curve.

### Resources to learn Rust

There are already great official resources to learn Rust, for instance:
- [The Rust Programming Language book](https://doc.rust-lang.org/book/)
- [Rust By Example](https://rustbyexample.com/)
- [Frequently Asked Questions](https://www.rust-lang.org/en-US/faq.html)

There is also a [large collection of community-maintained resources](https://github.com/ctjhoa/rust-learning), and the following are some resources that are possibly useful:
* [Comparison with other languages](https://github.com/ctjhoa/rust-learning#comparison-with-other-languages)
* [Cheat sheets](https://github.com/ctjhoa/rust-learning#cheat-sheets)
* [Ownership / Concurrency](https://github.com/ctjhoa/rust-learning#ownership--concurrency)

More resources: [Rust Official Documentation](https://www.rust-lang.org/en-US/documentation.html)

# Why not Rust

Choosing the right language for the job is important so let's look at a few possible reasons not to use Rust:
- [The ecosystem is not as mature](https://github.com/ctjhoa/rust-learning#are-we--yet)
- Rust's compiler is extremely strict in that it enforces ownership, i.e. needs time getting used to. However, it is [probably debatable if this approach is a worthwhile trade-off](https://news.ycombinator.com/item?id=16202373).
- While not a completely similar language, [Go feels immediately more productive](https://news.ycombinator.com/item?id=13430108)

Some other discussions on why not Rust:
- [How can Rust improve](https://www.reddit.com/r/rust/comments/7p75ab/why_rust_what_i_want_changed_for_rust_to_help_way/)
- [Why not Rust](https://www.reddit.com/r/rust/comments/6hp54n/blog_why_not_to_use_rust/)
