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

* [NumPy's Purpose](#numpys-purpose)
* [NumPy's Benefits and Drawbacks](#numpys-benefits-and-drawbacks)
* [Internals of NumPy](#internals-of-numpy)
  * [The Array Type](#the-array-type)
  * [Library Functions](#library-functions)
* [Tutorials and Resources](#tutorials-and-resources)
</box>

<box type="info">

Before reading this article, you should have basic knowledge of Python and its iterable types.
If not, do read [the introduction to Python](introduction-to-python.html) included among these learning resources. 
</box>

## NumPy's Purpose

Data scientists often work with highly structured data, like time series or plots of pixel values. On these data sets,
they routinely need to perform operations uniformly and efficiently. Without external packages, Python can express
and handle some of these operations reliably. For huge sets, however – like the training and test data provided to
machine learning algorithms – Python becomes complicated and sluggish.

[NumPy](https://numpy.org) fills in the gaps, simplifying (and speeding up) the code required to process and
visualise data sets large and small. So many other science-oriented Python libraries depend on NumPy (pandas,
matplotlib, etc.) that NumPy describes itself as "the fundamental package for scientific computing in Python".

## NumPy's Benefits and Drawbacks

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
a primitive C type or something similar to one, for the rest of NumPy to work well; in particular Python integers
are treated as generic Python objects if they are too large, negating speed improvements, while strings have to be
interpreted as character arrays.
```python
>>> np.array([1.5, "a"])
array(['1.5', 'a'], dtype='<U32') # not a numeric type!
>>> np.array([10 ** 20, 1])
array([100000000000000000000, 1], dtype=object)
```

Another limitation occurs when resizing arrays. In most cases, enlarging an array creates a new copy, like in C,
rather than simply making space for the new cells like in Python. Memory has to be managed manually for code that
works with data at scale.
```python
>>> A = np.array([[5, 0], [-1, -3]])
>>> B = np.append(A, [[-3], [1]], axis=1) # creates a new object
>>> B[0,0] += 1
>>> A
array([[ 5,  0],
       [-1, -3]]) # not modified
```

## Internals of NumPy

Although NumPy overall is an intricate language, you should be able to easily grasp its two main features:
C-like arrays and the functions that work on them.

### The Array Type

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

### Library Functions

In addition to its array type, NumPy provides a large [library](https://numpy.org/devdocs/reference/routines.html)
for manipulating arrays. These include:
* [creating special types of arrays](https://numpy.org/devdocs/reference/routines.array-creation.html)
* [sorting and searching](https://numpy.org/devdocs/reference/routines.sort.html)
* [random number generation](https://numpy.org/devdocs/reference/random/index.html)
* [linear algebra](https://numpy.org/devdocs/reference/routines.linalg.html)
* [other mathematical functions](https://numpy.org/devdocs/reference/routines.math.html)
```python
# Example: solve a system of linear equations with NumPy
>>> A = np.array([[9, -9, -10], [8, 5, -4], [0, 7, -9]])
>>> b = np.array([-3, 5, -7])
>>> np.linalg.solve(A, b)
array([0.95958854, 0.22997796, 0.95664952])
```
Many of these functions work equally well on one-dimensional arrays as they do on 11-dimensional ones due to the
compiled binaries. Many can take in standard Python nested lists, automatically converting them to NumPy's
array type before proceeding. Some functions also accept extra arguments that refine the parts of arrays they work on
or the values they return, allowing finer control beyond indexing and slicing. The `axis` argument in `np.mean`
[above](#numpys-benefits-and-drawbacks) is an example.

For even faster programs, it is possible to interact with external C libraries through NumPy's
[application programming interface](https://numpy.org/devdocs/reference/c-api/index.html), but this is not needed
for general use. The primary reason for exposing such a functionality is the continuing widespread use of C libraries
in mathematics and physics.

## Tutorials and Resources

Where can you get started with learning NumPy, then? The [NumPy User Guide](https://numpy.org/devdocs/user/index.html)
contains several tutorials for programmers of all abilities, including the following:
* [NumPy: the absolute basics for beginners](https://numpy.org/devdocs/user/absolute_beginners.html) introduces the
array type.
* [NumPy basics](https://numpy.org/devdocs/user/basics.html) goes into slightly more advanced topics, such as
broadcasting (the mechanism for interpreting operations between differently sized arrays, or between arrays
and scalars).
* The [quickstart](https://numpy.org/devdocs/user/quickstart.html) encompasses both the above tutorials, fits on
one page and also has handy links to commonly used functions. 
* A tutorial on doing [some linear algebra with NumPy](https://numpy.org/devdocs/user/tutorial-svd.html).
* [A cheat sheet](https://numpy.org/devdocs/user/numpy-for-matlab-users.html) for programmers coming from MATLAB,
another array-based programming language.

----

In practice, NumPy is not often used alone, but rather as part of what NumPy's documentation calls a _scientific
Python distribution_, which also includes several NumPy dependents. The most widely used such distribution is
[Anaconda](https://www.anaconda.com/distribution).

Some of these dependents are listed below, with links to tutorials that themselves use NumPy to drive their high-level
routines. Reading them may illuminate your journey in understanding and the underlying array language.
* [matplotlib](https://matplotlib.org/tutorials/index.html): produces plots of all kinds. Input to the plotting
functions is typically in the form of NumPy arrays.
* [SciPy](https://docs.scipy.org/doc/scipy/reference/tutorial/general.html): essentially a more elaborated NumPy
with computationally intensive routines for tasks such as optimisation and image processing.
* [scikit-learn](https://scikit-learn.org/stable/tutorial/basic/tutorial.html): like SciPy, but for machine learning.
* [pandas](https://pandas.pydata.org/docs/getting_started/10min.html): uses NumPy in its `DataFrame` object.
* [Numba](https://numba.pydata.org/numba-doc/latest/user/5minguide.html): heavily relies on the contingency offered
by NumPy objects to enable massively parallel computation.
</div>
