# Code Quality Metrics

Authors: [Wilson Kurniawan](https://github.com/wkurniawan07)

## Overview

- It is undisputed that developers, code reviewers, testers, QA team members, and software architects alike want code to be as simple, understandable, and (put more positive adjectives here) as possible.
- It is, however, disputable how "simple", "understandable", etc. is defined, because they are all qualitative. What is "..." for one person may not be so "..." for another; in fact, it may not be so for that person three months later.

A quantitative approach needs to be adopted to measure those qualities precisely. **Code Quality Metrics** is the answer. It provides a **quantitative measure** of the quality of a piece of code.

> Since quantitative measurements are essential in all sciences, there is a continuous effort by computer science practitioners and theoreticians to bring similar approaches to software development.
> The goal is obtaining objective, reproducible and quantifiable measurements, which may have numerous valuable applications in schedule and budget planning, cost estimation, quality assurance testing, software debugging, software performance optimization, and optimal personnel task assignments.
>
> https://en.wikibooks.org/wiki/Introduction_to_Software_Engineering/Quality/Metrics

In fact, [the Boeing 777 project attributed its success to excellent usage of Quality Metrics (both code and non-code)](https://thesai.org/Downloads/Volume3No1/Paper%2021-Survey%20on%20Impact%20of%20Software%20Metrics%20on%20Software%20Quality.pdf).

*Note that the term "Code Quality" here is scoped to be the readability, understandability, and maintainability of the code. While performance can be considered a legitimate basis for some metrics (e.g. time taken to run certain operations, based on profiling), it is not considered for this purpose.*

## Examples of Metrics

### Complexity Metrics

Complexity metrics measure how "complex" methods, classes, packages, etc. are. "Complex" here is defined as difficult to understand and difficult to maintain.

They are arguably the most useful metrics for the largest number of developers because they are the easiest to grasp, the most directly relevant to the coding activity, and applicable to most/all programming languages.

- The most well-known complexity metric is **Cyclomatic Complexity (CC)**, invented by Thomas McCabe in 1976. CC corresponds to the **minimum number of test cases needed to achieve 100% branch coverage**. [Here](http://www.whiteboxtest.com/cyclomatic-complexity.php) is an excellent explanation on how to make use of it to measure your code quality.
- An alternative, less well-known, harder to calculate complexity metric is **NPath Complexity (NC)**, invented by Brian A. Nejmeh in 1988. NC corresponds to the **minimum number of test cases needed to achieve 100% path coverage**.

> [Here](http://www.literateprogramming.com/mccabe.pdf) is the original paper by McCabe, and [here](http://dl.acm.org/citation.cfm?doid=42372.42379) is the original paper by Nejmeh.

These complexity metrics can be extended to class level (e.g. summing or averaging the complexity values of all methods in a class), package level, or even project level.

### Class Design Metrics

Class design metrics measure how "well-designed" a class is. "Well-designed" here is defined as conformance to good software engineering principles such as high cohesion, low coupling, promoting encapsulation and information hiding. Well-designed classes promote reuse and ease maintenance effort.

Metrics on this level are more applicable for QA team members and software architects, but still hold some relevance to students and junior developers who have to design a class-level API.

The most well-accepted metrics for class design are [**The Chidamber and Kemerer Metrics**](http://www.virtualmachinery.com/sidebar3.htm):
- Weighted Methods per Class (WMC)
- Coupling Between Objects (CBO)
- Response For Class (RFC)
- Lack of Cohesion of Methods (LCOM)
- Depth of Inheritance Tree (DIT)
- Number of Children (NOC)

### Package Design Metrics

Package design metrics measure how "well-designed" a package is. "Well-designed" here is similarly defined as the one in class design metrics. Metrics on this level are mostly applicable only for QA team members and software architects.

The most well-accepted metrics for package design are [**"the group of five"**](http://www.virtualmachinery.com/jhawkmetricssyspack.htm):
- Afferent Coupling (Ca)
- Efferent Coupling (Ce)
- Instability (I)
- Abstractness (A)
- Distance from Main Sequence (D)

### Seemingly Trivial Metrics

The previous sections have introduced mostly new concepts that are foreign to many developers.
However, measures that are seemingly trivial and easy-to-overlook can also count as metrics, such as the following:
- Number of lines of codes in a class/method/...
- Number of methods in a class
- Number of fields in a class
- Number of parameters in a method
- Nesting depth of a method
- Code coverage
- Percentage of comments
- Percentage of code duplication

## Define Your Own Metric

You would have noticed that these metrics are self-formulated rather than being inherent properties of a piece of software, and you are right.

You might then ask: can I define my own code quality metric? Yes, you can. [This page](http://www.developer.com/tech/article.php/3644656/Software-Quality-Metrics.htm) has a good guide on how to formulate your own metric.

## Making Sense of It

Any code quality metric is as good as how it is used; without context, it is merely a number. **Do not** use any metric just for the sake of it.
- It makes little sense to say: "This method has a CC value of 42. It is bad."
- It makes more sense to say: "This method has a CC value of 42. That means we need at least 42 test cases to achieve full branch coverage for it, and it is bad."
- It makes the most sense to say: "This method has a CC value of 42. That means we need at least 42 test cases to achieve full branch coverage for it. This indicates an overly complex method, and it will be difficult to maintain in the long run. Try to separate it to calls of a few methods of CC value of 10 or less each; that way we can better design test cases for those smaller methods and achieve perfect coverage."

[Here](http://homepages.dcc.ufmg.br/~figueiredo/disciplinas/lectures/detection-strategy-examples_v01.pdf) is an excellent example of how different metrics are used to determine which classes/methods need some maintenance works.

## Code Quality Metrics Tools

Measuring complexity (relevant to most developers):
- Java: [PMD](https://pmd.github.io)
- JavaScript: [ESLint](http://eslint.org)
- C#: [Visual Studio Code Analysis](https://blogs.msdn.microsoft.com/zainnab/2011/05/17/code-metrics-cyclomatic-complexity/)
- Python: [radon](https://pypi.python.org/pypi/radon)
- Ruby: [RuboCop](http://batsov.com/rubocop/)
- PHP: [PHPMD](https://phpmd.org)

Measuring design (relevant to architects, QA team), most of them commercial:
- Java: [JArchitect](http://www.jarchitect.com)
- C#: [NDepend](http://www.ndepend.com)
- PHP: [PHP Depend](https://pdepend.org)
