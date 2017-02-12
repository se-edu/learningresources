# An Introduction to Python

Python is a simple yet powerful and versatile language. Conceived in the late 80s, it is now widely used across many fields of computer science and software engineering. While not as speedy as compiled languages like C or Java, Python's emphasis on readability, and resulting ease of maintenance, often outweighs the advantages conferred by compiled languages. This especially true in applications where execution speed is non-critical.

## Getting started with Python

If you're reading this on GitHub, I'm guessing that you're already an experienced programmer looking to get in on the Python action. If that's the case, check out [Google's Python class](https://developers.google.com/edu/python/), which will introduce you to Python's concepts and core data structures like lists, dictionaries, and strings!

For absolute beginners, check out [this video](https://www.youtube.com/watch?v=N4mEzFDjqtA) by Derek Banas where he covers everything from installing Python and the basics to Inheritance and Polymorphism(!!!) in under an hour! If you'd prefer to read, check out [Python Guru](http://thepythonguru.com/) which has plenty of code samples to help you along.

Both newbies and experienced programmers can also benefit from [The Python Tutorial](https://docs.python.org/3/tutorial/index.html), which aims to introduce readers to Python's unique features and style.

## Iterables

Strings, lists, and dictionaries belong to a type of class known as `Iterables`. If you're interested in delving nto the inner workings of `Iterable` classes, check out [Iterables, Iterators and Generators](https://excess.org/article/2013/02/itergen1/) by Ian Ward. He begins by explaining the `Iterable` class, then goes into the `Iterator` and `Generator` classes, both of which are powerful tools in Python.

## Indexing and Slicing

Accessing an element in a list using its index is known as indexing, e.g. `my_list[0]` returns the first element of my_list (Iterables are zero-indexed; this means that the first index is 0). Slicing, on the other hand, allows us to access a range of elements in a list. Extended slicing extends this functionality by introducing a step parameter, allowing us to do things like access every other element. Read more about this inn my blog post on [indexing and slicing](https://samsontmr.github.io/Slicing-and-Dicing/) in Python,

## Object Oriented Programming in Python

While Python is really useful for writing small scripts, often we want to use it to write whole applications. When this happens, having everything in one script (we call this procedural programming) will create huge problems readability-wise, especially when things start breaking and you're trying to find out why! This is where Object Oriented Programming comes in, offering a way to think about your program and organizing it into readable chunks. Jeff Knupp gives an introduction to [Python Classes and OOP](https://jeffknupp.com/blog/2014/06/18/improve-your-python-python-classes-and-object-oriented-programming/) here.


## Python for Data Science

Data Science seems to be all the rage recently, so you'll be glad to know that Python has become a major player in the field of Data Science, rivalled only by R. Some commonly used Python libraries in Data Science include [pandas](http://pandas.pydata.org/) for data management and manipulation, [numpy](http://www.numpy.org/) and [scipy](http://www.scipy.org/) for scientific computing, [scikit-learn](http://scikit-learn.org/) for general machine learning and [matplotlib](http://matplotlib.org/) for data visualization.

### Tutorials

To get started, take a look at my [introduction to pandas](https://samsontmr.github.io/Sentimental-Pandas/), the [numpy Quickstart](https://docs.scipy.org/doc/numpy-dev/user/quickstart.html) guide, [matplotlib tutorials](http://matplotlib.org/users/tutorials.html#introductory) or
[scikit-learn basics](http://scikit-learn.org/stable/tutorial/basic/tutorial.html). scikit-learn also has a number of machine learning focused [tutorials](http://scikit-learn.org/stable/tutorial/index.html) available for those looking to pick up machine learning.