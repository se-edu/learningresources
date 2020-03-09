<frontmatter>
  title: NumPy
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# NumPy

Author: [Jeremy Tan](https://github.com/Parcly-Taxel)

<box id="article-toc">

* [Benefits and Drawbacks](#benefits-and-drawbacks)
* [The Array Type](#the-array-type)
* [Library Functions](#library-functions)
* [Tutorials and Resources](#tutorials-and-resources)
</box>

<box type="info">

Before reading this article, you should have basic knowledge of Python and its iterable types.
If not, do read [the introduction to Python](introduction-to-python.html) included among these learning resources. 
</box>

Data scientists often work with highly structured data, like time series or plots of pixel values. On these data sets,
they routinely need to perform operations uniformly and efficiently. Without external packages, Python can express
and handle some of these operations reliably. For huge sets, however – like the training and test data provided to
machine learning algorithms – Python becomes complicated and sluggish.

[NumPy](https://numpy.org) fills in the gaps, simplifying (and speeding up) the code required to process and
visualise data sets large and small. So many other science-oriented Python libraries depend on NumPy (pandas,
matplotlib, etc.) that NumPy describes itself as "the fundamental package for scientific computing in Python".

## Benefits and Drawbacks

NumPy's main benefit is its abstraction of arrays. In a traditional imperative style, handling multi-dimensional
arrays requires multiple for loops or list comprehensions; both approaches lead to tedious, error-prone code, and
do not take advantage of vector instructions in modern processors.

For example, to find the average of each column in a two-dimensional array (list of lists):
```python
def find_averages(arr):
    l = len(arr[0])
    m = len(arr)
    return max(sum(arr[j][i] for j in range(m)) / m for i in range(l))
```
This is quite a lot of work for a simple task. The equivalent code in NumPy is a single call:
```python
import numpy as np
np.mean(arr, 0)
```
If you frequently deal with data sets more complicated than one-dimensional lists, NumPy is suited for you.

Transcribing array-based mathematics into NumPy is also made simpler by the many array functions and operators
in its library. Not only is the code clearer – a Python virtue – but it can reduce development and debugging time.
```python
# A and B are 2-by-2 matrices, c is a scalar
A = np.array([[1, 5], [-1, 3]])
B = np.array([[0, 2], [2, -3]])
c = 2

c * A # scalar multiplication
A @ B # matrix multiplication
A > B # compare A and B elementwise
B.T # matrix transpose
```

The downside to these benefits is a large memory footprint: NumPy's performance depends on compiled C and Fortran code,
so it may not be suitable for embedded systems. All elements of a NumPy array must also be of _the same_ type,
a primitive C type or something similar to one, for the rest of NumPy to work well; in particular Python's
arbitrary-precision integers are treated as generic Python objects if they are too large.
```python
>>> np.array([1.5, "a"])
array(['1.5', 'a'], dtype='<U32') # not a numeric type!
>>> np.array([10 ** 20, 1])
array([100000000000000000000, 1], dtype=object)
```

## The Array Type

NumPy is built around its `array` type (an alias for `ndarray`, hinting at its multidimensional nature). Unlike Python
lists, elements of `array` instances are stored in memory contiguously, reducing interpreter overhead
and enabling parallel operations.

An `array` object can be initialised from iterables or nested lists through `np.array` (assuming NumPy has been
imported as `np`, as is usually done in production code, and which will be assumed in the rest of this article):
```python
a = np.array([7, 3, 9, 3, 1, 3, 3, 7]) # 1D array
b = np.array([[8, 1, 6], [3, 5, 7], [4, 9, 2]]) # 2D array
```
By default, these arrays are indexed in C order (last index changes fastest), as can be illustrated by
invoking the `reshape` array method:
```python
>>> np.array(range(27)).reshape((3,3,3))
array([[[ 0,  1,  2],
        [ 3,  4,  5],
        [ 6,  7,  8]],

       [[ 9, 10, 11],
        [12, 13, 14],
        [15, 16, 17]],

       [[18, 19, 20],
        [21, 22, 23],
        [24, 25, 26]]])
```
Otherwise, indexing is mostly as in standard Python, with the convenience that extra square brackets can be
omitted for multi-_axis_ (array coordinate) indexing, so `a[0][1:][2]` becomes `a[0,1:,2]`.
See [this page](https://numpy.org/devdocs/reference/arrays.indexing.html) for a more detailed explanation.

## Library Functions

In addition to its array type, NumPy provides a large [library](https://numpy.org/devdocs/reference/routines.html)
for manipulating arrays. These include:
* [creating special types of arrays](https://numpy.org/devdocs/reference/routines.array-creation.html)
* [sorting and searching](https://numpy.org/devdocs/reference/routines.sort.html)
* [random number generation](https://numpy.org/devdocs/reference/random/index.html)
* [string operations](https://numpy.org/devdocs/reference/routines.char.html), with strings treated as character arrays
* [linear algebra](https://numpy.org/devdocs/reference/routines.linalg.html) and other
[mathematical functions](https://numpy.org/devdocs/reference/routines.math.html)

```python
# Example: solve a system of linear equations with NumPy
>>> A = np.array([[9, -9, -10], [8, 5, -4], [0, 7, -9]])
>>> b = np.array([-3, 5, -7])
>>> np.linalg.solve(A, b)
array([0.95958854, 0.22997796, 0.95664952])
```

Many of these functions work equally well on one-dimensional arrays as they do on 11-dimensional ones, and their
scope can be refined by passing axis arguments as in the [array average example](#what-numpy-can-and-cannot-do-for-you)
above. In some cases, the function is accelerated further through
[universal functions](https://numpy.org/devdocs/reference/ufuncs.html), vectorised C code with thin Python wrappers.

Properly written NumPy code will spend most of its time in the compiled binaries, only returning to the Python layer
for input and output. In this way, you get the speed advantages of C/Fortran and the convenience of Python together.  

## Tutorials and Resources

The [NumPy User Guide](https://numpy.org/devdocs/user/index.html) contains several tutorials for programmers of all
abilities, including the following:
* [NumPy: the absolute basics for beginners](https://numpy.org/devdocs/user/absolute_beginners.html) introduces the
array type.
* [NumPy basics](https://numpy.org/devdocs/user/basics.html) goes into slightly more advanced topics, such as
broadcasting (the mechanism for interpreting operations between differently sized arrays, or between arrays
and scalars).
* The [quickstart](https://numpy.org/devdocs/user/quickstart.html) encompasses both the above tutorials, fits on
one page and also has handy links to commonly used functions. 
* A tutorial on doing [some linear algebra with NumPy](https://numpy.org/devdocs/user/tutorial-svd.html).
* [A cheat sheet](https://numpy.org/devdocs/user/numpy-for-matlab-users.html) for programmers coming from MATLAB,
another array-based (but proprietary) programming language.

In practice, NumPy is not often used alone, but rather as part of what NumPy's documentation calls a _scientific
Python distribution_, which includes all the NumPy dependents mentioned below in [Resources](#resources) and more.
The most widely used such distribution is [Anaconda](https://www.anaconda.com/distribution).

Beyond the tutorials above and the [reference of functions](https://numpy.org/devdocs/reference/index.html), most major
projects that depend on NumPy have their own tutorials that themselves use NumPy to drive the methods under discussion.
Reading them may illuminate your journey in understanding and eventually mastering the underlying array language.
* [matplotlib](https://matplotlib.org/tutorials/index.html): produces plots of all kinds. Input to the plotting
functions is typically in the form of NumPy arrays.
* [SciPy](https://docs.scipy.org/doc/scipy/reference/tutorial/general.html): essentially a more elaborated NumPy
with computationally intensive routines for tasks such as optimisation and image processing.
* [pandas](https://pandas.pydata.org/docs/getting_started/10min.html): uses NumPy in its `DataFrame` object.
</div>
