<frontmatter>
  title: Introduction to Ruby
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

# Introduction to Ruby

Authors: [Wilson Kurniawan](https://github.com/wkurniawan07)

## Ruby Overview

**Ruby** is a **dynamic**, **reflective**, **object-oriented**, **general-purpose** programming-language, designed and developed by Yukihiro "Matz" Matsumoto in 1990s.
It is widely used across many fields of computer science and software engineering, and its most prevalent use is in web development.

Ruby boasts high programmer productivity with its concise, elegant, and human-readable syntax, which is widely agreed to outweigh the loss in execution speed (as compared to compiled languages such as C and Java) unless the latter is critical.

> Often people, especially computer engineers, focus on the machines. They think, "By doing this, the machine will run faster. By doing this, the machine will run more effectively. By doing this, the machine will something something something."
> They are focusing on machines. But in fact, we need to focus on humans, on how humans care about doing programming or operating the application of the machines. We are the masters. They are the slaves.
> 
> *Yukihiro Matsumoto, creator of Ruby language* 

Popular websites running on Ruby: GitHub, Airbnb, Twitter, Shopify.

## Getting Started

[The official website of Ruby](https://www.ruby-lang.org/en/) will have everything you need from release notes to downloads to documentations.

To install Ruby, go to the download page of the above website and follow the instructions.
If you're a Mac user, you don't need to do anything; Macs come with Ruby pre-installed!

For beginners and intermediate users, [tutorialspoint](https://www.tutorialspoint.com/ruby/index.htm) has great introductory Ruby tutorials, and [rubylearning.com](http://rubylearning.com/satishtalim/tutorial.html) is another great place.
We will explore some of the more prominent language features of Ruby.

### Everything is an object

Ruby has no concept of "primitives". Anything that can be assigned to a variable is an object. Even numbers and boolean values `true` and `false` are objects.

```rb
5.times do
  puts "Hello world!"
end

# Hello world!
# Hello world!
# Hello world!
# Hello world!
# Hello world!
```

As such, it becomes very easy to introduce new functions to those "primitives":

```rb
class Fixnum # integers in Ruby belong to this class
  def next()
    return self + 1
  end
end

7.next()
#  => 8
```

### Readability is king

> Ruby is simple in appearance, but is very complex inside, just like our human body.
> 
> *Yukihiro Matsumoto*

Some languages attempt to make their codes as close to pseudocode as possible, but Ruby takes it to another level: Ruby code flows almost as smoothly as an English literary text.

```rb
for num in 0..5
  puts "I am counting #{num}!"
end

unless number.even?
  puts "Number is odd!"
end

["chocolate", "strawberry", "vanilla"].each { |flavour|
  make_ice_cream(flavour)
}
```

*No, you're not reading an English poem. You're reading Ruby!*

### Functional programming is encouraged

For those familiar with [higher-order functions](http://www.cse.unsw.edu.au/~en1000/haskell/hof.html), Ruby supports *map*, *fold-left*, and *filter*.

```rb
# Map
[1, 2, 3, 4, 5].map { |n| n * n }
#  => [1, 4, 9, 16, 25]

# Fold-left
[1, 2, 3, 4, 5].inject(0) { |sum, n| sum + n }
#  => 15

# Filter
[1, 2, 3, 4, 5].select { |n| n.even? }
#  => [2, 4]
```

Joel McCracken makes an [excellent short presentation](http://joelmccracken.github.io/functional-programming-in-ruby/#/) on how functions are treated in Ruby.

### Object-oriented programming is possible, too

```rb
class Circle
  def initialize(r)
    @radius = r
  end

  def get_radius
    return @radius
  end

  def get_area
    return Math::PI * @radius * @radius
  end
end

c = Circle.new(5)
c.get_area
#  => 78.53981633974483
```

More on this in [Object-oriented Ruby tutorial](https://www.tutorialspoint.com/ruby/ruby_object_oriented.htm).

## Advanced Topics

- [Blocks in Ruby](https://www.tutorialspoint.com/ruby/ruby_blocks.htm) - ever imagine that a method invocation can be another method's parameter?
- [Modules and mixins in Ruby](https://www.tutorialspoint.com/ruby/ruby_modules.htm) - provides namespacing, and makes multiple inheritance (or a variant of it, technically) possible.
- [Threads and fibers in Ruby](http://pltconfusion.com/concurrency_primitives_and_abstractions_in_ruby/) - dealing with concurrencies.
- [Metaprogramming in Ruby](https://www.toptal.com/ruby/ruby-metaprogramming-cooler-than-it-sounds) - when your Ruby code *writes* Ruby code at runtime.
- [Domain-Specific Languages (DSLs) in Ruby](https://www.leighhalliday.com/creating-ruby-dsl) - Ruby's amazing support for blocks and metaprogramming makes it a first choice for many developers to write a DSL.
- [Ruby style guide](https://github.com/bbatsov/ruby-style-guide) - as agreed by the community at large.
- [21 Ruby tricks](http://www.rubyinside.com/21-ruby-tricks-902.html) - making good use of Ruby's language features.
- [Ruby best practices](http://www.reedbushey.com/119Ruby%20Best%20Practices.pdf) - a book written by a Ruby expert with foreword provided by Yukihiro Matsumoto himself.

## Ruby Frameworks and DevOps

As with most other languages, tools and frameworks exist for serious Ruby developers and project managers to assist many of their tasks.

- **Frameworks:** [Ruby on Rails](http://rubyonrails.org) is by far the most popular web application framework for Ruby. Another popular framework is [Sinatra](http://www.sinatrarb.com).
- **Libraries:** Ruby libraries come in form of *gems*. The complete registry can be found on [this website](https://rubygems.org).
- **IDE:** [Aptana Studio](http://www.aptana.com/products/studio3.html) is the favourite IDE for Rails developers. Other alternatives are [RubyMine](https://www.jetbrains.com/ruby/) (commercial) and Eclipse with [RDT plugin](https://sourceforge.net/projects/rubyeclipse/).
- **Task Automation:** [Rake](http://docs.seattlerb.org/rake/) is the most commonly used automation tool.
- **Static Analysis:** [RuboCop](http://batsov.com/rubocop/) is the sole leading static analysis tool for Ruby language.

[This repository](https://github.com/markets/awesome-ruby) lists down a large collection of Ruby libraries, tools, frameworks, and software.

</div>
