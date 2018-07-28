<frontmatter>
  title: An Introduction to Python
  footer: footer.md
  head: head.md
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# An Introduction to Python

Authors: [Samson Tan Min Rong](https://www.linkedin.com/in/samsontmr/), [Phang Chun Rong](https://www.github.com/Crphang)

Python is a simple yet powerful and versatile language. Conceived in the late 80s, it is now widely used across many fields of computer science and software engineering. While not as speedy as compiled languages like C or Java, Python's emphasis on readability, and resulting ease of maintenance, often outweighs the advantages conferred by compiled languages. This especially true in applications where execution speed is non-critical.

## Getting started with Python

If you're a programmer looking to get in on the Python action, check out [Google's Python class](https://developers.google.com/edu/python/), which will introduce you to Python's concepts and core data structures like lists, dictionaries, and strings!

For absolute beginners, check out [this video](https://www.youtube.com/watch?v=N4mEzFDjqtA) by Derek Banas where he covers everything from installing Python and the basics to more advanced concepts like Inheritance and Polymorphism in under an hour! If you'd prefer to read, check out [Python Guru](http://thepythonguru.com/) which has plenty of code samples to help you along.

Both newbies and experienced programmers can also benefit from [The Python Tutorial](https://docs.python.org/3/tutorial/index.html), which aims to introduce readers to Python's unique features and style.

## Python 2 vs Python 3

Despite obvious similarities in two Python versions, Python 3's intentional backward incompatibility makes choosing to learn Python 3 over Python 2 a tough problem for both new and experienced Python programmers. Some concerns include the lack of popular Python 2 packages in Python 3 as well as changes in some Python built-in libraries that might break existing systems. One example is that a simple `print 'Hello World'` that runs perfectly in Python 2 will cause a syntax error in Python 3.

Read more in this Digital Ocean's [post](https://www.digitalocean.com/community/tutorials/python-2-vs-python-3-practical-considerations-2) to understand this conundrum of choosing between Python 2 and Python 3 better. If you choose to learn both Python 2 and Python 3, take a look at some of these important [changes](https://www.geeksforgeeks.org/important-differences-between-python-2-x-and-python-3-x-with-examples/) to avoid [gotchas](#gotchas) due to version differences.

Overall, it is recommended to learn Python 3 as a beginner today as it has been [10 years since Python 3 debuted](https://www.python.org/download/releases/3.0/). This stance is also [supported](http://python-notes.curiousefficiency.org/en/latest/python3/questions_and_answers.html#why-is-python-3-considered-a-better-language-to-teach-beginning-programmers) by core members of Python. Moreover, unlike the early days of Python 3's release, many popular packages from Python 2 now support Python 3 as well. Since Python 2 is a legacy language while Python 3 is in active development, it would be better to learn Python 3 today.

## Virtual Environment

When starting new projects or hopping onto existing Python repositories, you are recommended to install dependencies using a [virtual environment](https://docs.python.org/3/tutorial/venv.html) to avoid dependency conflicts. This is a good practice especially when managing dependencies from different projects which may rely on different Python versions and packages.

The [official Python documentation](https://docs.python.org/3/tutorial/venv.html) gives instructions on the standard way of creating a virtual environment - defining a directory location and activating it. However, you can consider using other libraries that can make this process smoother. Some of the most popular ones are:

* [virtualenvwrapper](http://virtualenvwrapper.readthedocs.io/en/latest/install.html) - Allows you to define all environments in a single place instead of having to manage the different environments in your local system.
* [pyenv](https://github.com/pyenv/pyenv) and [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv). `pyenv` allows easy management of different Python versions and `pyenv-virtualenv` allows managing virtualenv associated to the Python versions managed by `pyenv`.

## Common Data Structures

Strings, lists, and dictionaries belong to a type of class known as `Iterables`. An `Iterable` is defined by Python to be "an object capable of returning its members one at a time".

Due to their versatility you'll often find that strings, lists, and dictionaries are all you'll ever need. However, there may come a time when you'll need to create your own `Iterable` data structure. If that's the case, you may want to delve into the inner workings of `Iterable` classes, check out [Iterables, Iterators and Generators](https://excess.org/article/2013/02/itergen1/) by Ian Ward. He begins by explaining the `Iterable` class, then goes into the `Iterator` and `Generator` classes, both of which are powerful tools in Python.

## Indexing and Slicing

Accessing an element in a list using its index is known as indexing, e.g. `my_list[0]` returns the first element of my_list (Iterables are zero-indexed—their first index is 0). Slicing, on the other hand, allows us to access a range of elements in a list. Extended slicing extends this functionality by introducing a step parameter, enabling operations like accessing every other element. Read more about this in a blog post on [indexing and slicing](https://samsontmr.github.io/Slicing-and-Dicing/) in Python! There, I also explain the basics of list comprehension, a type of syntactic sugar for transforming lists. Trey Hunner gives an in-depth explanation [here](http://treyhunner.com/2015/12/python-list-comprehensions-now-in-color/) using the for-loops we all know and love.

## Object Oriented Programming in Python

While Python is really useful for cranking out small scripts, we often want to use it to build fully fledged applications too. When this happens, [programming procedurally](https://en.wikipedia.org/wiki/Procedural_programming) results in major readability and maintenance issues—especially when your code starts breaking and you're trying to figure out why!

This is where Object Oriented Programming comes in, offering a way to think about your program and organizing it into readable chunks. Jeff Knupp gives an introduction to [Python Classes and OOP](https://jeffknupp.com/blog/2014/06/18/improve-your-python-python-classes-and-object-oriented-programming/) here, introducing concepts like static vs instance attributes, inheritance, abstract classes, and the Liskov Substitution Principle.

## Functional Programming

In addition to scripting and OOP, Python also supports [functional programming](https://medium.com/@cscalfani/so-you-want-to-be-a-functional-programmer-part-1-1f15e387e536#.70kgem2gc), a completely different paradigm from OOP and procedural programming. Where procedural programming and OOP makes heavy use of states, functional programming eschews them completely. In this paradigm, the result of a function is wholly dependent on its arguments.

*"Why would I want to relearn how to code?!"*, you may ask. Big Data's increasing relevance has thrown the spotlight on distributed systems and concurrency techniques. No prizes for guessing which programming paradigm is most amenable to the implementation of these systems. [Here](http://www.python-course.eu/python3_lambda.php) is a great introduction to the tools at the heart of functional programming in Python 3: `map`, `filter`, `reduce`, and `lambda`.

An alternative way of doing functional programming in Python using list comprehensions to replace the `map`, `filter`, and `reduce` functions. [Here](http://www.u.arizona.edu/~erdmann/mse350/topics/list_comprehensions.html) is a comparison of both methods, complete with examples!

## Python for Data Science

Data Science seems to be all the rage recently, so you'll be glad to know that Python has become a major player in the field of Data Science, rivalled mainly by R. Some commonly used Python libraries in Data Science include [pandas](http://pandas.pydata.org/) for data management and manipulation, [numpy](http://www.numpy.org/) and [scipy](http://www.scipy.org/) for scientific computing, [scikit-learn](http://scikit-learn.org/) for general machine learning and [matplotlib](http://matplotlib.org/) for data visualization.

### Tutorials

To get started, take a look at:
*   [Introduction to pandas](https://samsontmr.github.io/Sentimental-Pandas/)
*   [Numpy Quickstart Guide](https://docs.scipy.org/doc/numpy-dev/user/quickstart.html)
*   [matplotlib Tutorials](http://matplotlib.org/users/tutorials.html#introductory)
*   [scikit-learn Basics](http://scikit-learn.org/stable/tutorial/basic/tutorial.html)
*   [scikit-learn Machine Learning tutorials](http://scikit-learn.org/stable/tutorial/index.html) are also available for those looking to pick up machine learning.
    *   If you learn best by working on projects, fret not! [Kaggle](http://kaggle.com) is a treasure trove of real-world datasets that you can hone your data science skills on.

## Gotchas

Like most other languages, Python has its own set of common gotchas that can really frustrate newbie Python programmers due to the unintended bugs. Let's consider this common pitfall that many Python programmers encounter.

```Python
def append_to(element, to=[]):
    to.append(element)
    return to

my_list = append_to(12)
print(my_list) # [12]

my_other_list = append_to(42)
print(my_other_list) # [12,42]
```

Looking at the above example, one might think that `my_other_list` will be `[42]` but actually is `[12,42]`. The reason is because Python's default arguments, in this case `to = []`, are evaluated only once when the function is defined.

Learning how to avoid such pitfalls is one huge step towards being a productive Python programmer. Here are some other guides that state some common gotchas and how to avoid them:

* [Top 10 Common Mistakes of Python Programmers](https://www.toptal.com/python/top-10-mistakes-that-python-programmers-make)
* [Common Python Gotchas](https://sopython.com/wiki/Common_Gotchas_In_Python)
</div>
