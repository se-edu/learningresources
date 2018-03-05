# FindBugs

Author: [Xiao Pu](https://nus-oss.github.io/cs3281-website/students/AY1617S2/xiaoPu/xiaoPu-Resume.html), [Shradheya Thakre](https://github.com/tshradheya)

## Overview

FindBugs is a static analysis tool to find bugs in **Java** programs. It looks for 'bug patterns' in the code and signals possible violations. Potential errors are classified in four ranks:
1. scariest
1. scary
1. troubling
1. of concern

This is a hint to the developer about their possible impact or severity. For example, the bug 'null pointer dereference' has the pattern — A program declares a non-nullable variable but assigns `null` to the variable somewhere and uses it later.

## Features

The "bug patterns" can be divided into nine groups:
1. Bad practice
1. Correctness
1. Experimental
1. Internationalization
1. Malicious code vulnerability
1. Multithreaded correctness
1. Performance
1. Security
1. Dodgy code

Refer to [FindBugs official documentation](http://findbugs.sourceforge.net/bugDescriptions.html) for a comprehensive list of bugs and the explanation of each bug.

FindBugs analyses bytecode in compiled Java `.class` file and checks multiple files at the same time. This is unlike [CheckStyle](checkStyle.md) or [PMD](PMD.md) which can only check files one by one and analyse Java source code, allowing FindBugs to spot errors that would have been missed by CheckStyle and PMD. For example, one of the bug patterns in FindBugs is `RCN: Redundant nullcheck of value known to be non-null`. FindBugs will analyse all the assignments to a particular variable in the code base and then check whether the `nullcheck` for the variable is redundant or not.

## Examples of Bugs that can be found using FindBugs

### Incorrectly overriding methods

Consider the following code:

``` java
class Foo {
    //... data members ...
    //... methods ...

    //Overriding equals method - the wrong way
    public boolean equals(Foo foo) {
        //... logic ...
    }
}
```

In the above code, if `foo.equals()` method is called, the `equals()` method of `Object` class rather than `Foo` class will be called. This is due to the way the Java code resolves overloaded methods at compile-time. FindBugs warns the developer of possible cases when a class defines a co-variant version of the `equals()` or `compareTo()` method.

### Find hash-equals mismatch

The hashCode() and equals() method are called by many `Collection` based classes like - List, Maps, Sets, etc. FindBugs helps in finding problems when a class **overrides the `equals()` but not the `hashCode()` method or vice-versa**. Overriding only one of the `equals()` or `hashCode()` method can cause methods of Collection based classes to fail and hence FindBugs helps in reporting these errors at an early stage

### Return value of method ignored

FindBugs helps in finding places where your code has ignored the return value of method when it shouldn't have been.

``` java
1 String s = "bob";
2 s.replace('b', 'p');
3 boolean isCorrect = s.equals("pop"); //isCorrect is `false`
```

In the above examples, one would assume that the variable `isCorrect` is assigned `true` because the `line 2` replaces `b` with `p`. However since strings are immutable, the `replace()` function actually returns a new string with updated value rather than updating the string the method is called on.

Hence, `line 2` should be `String newString  = s.replace('b', 'p'); //newString ="pop"`

### Null pointer dereference

FindBugs looks for cases where a code path will or could cause a null pointer exception.

``` java
1  Person person = aMap.get("bob");
2  if (person != null) {
3      person.updateAccessTime();
4  }
5  String name = person.getName();
```

In the above example, the `aMap` may or may not contain "bob", so FindBugs will report *possible* `NullPointerException` at `line 5`

## How to use it

### Configuration

You can tell FindBugs which bug patterns to exclude and include by using [FilterFiles](http://findbugs.sourceforge.net/manual/filter.html). By default, if no filter files are provided, FindBugs will run all checks.

### Suppress Warnings

You can use filter files with `exclude` option in FindBugs as discussed above to suppress warnings.

In addition, you can also use `SuppressWarnings` [annotation](http://findbugs.sourceforge.net/manual/annotations.html) to filter out unwanted violations.

### Running

There are several ways to run FindBugs.

GUI:

- [Using the FindBugs GUI](http://findbugs.sourceforge.net/manual/gui.html)

Command Line:

- [Command Line](http://findbugs.sourceforge.net/manual/running.html)

Build Automation Tools:

- [Ant Task](http://findbugs.sourceforge.net/manual/anttask.html)
- [FindBugs Maven Plugin](http://gleclaire.github.io/findbugs-maven-plugin/)
- [Gradle FindBugs](https://docs.gradle.org/current/userguide/findbugs_plugin.html)

IDE Integration:

- [Eclipse Integration](http://findbugs.sourceforge.net/manual/eclipse.html)
- [IntelliJ Integration](https://plugins.jetbrains.com/plugin/3847-findbugs-idea)
- [NetBeans Integration](https://netbeans.org/kb/docs/java/code-inspect.html)

## SpotBugs - The successor of Findbugs

On November 2016, FindBugs was [declared dead](https://mailman.cs.umd.edu/pipermail/findbugs-discuss/2016-November/004321.html) and [SpotBugs](https://spotbugs.github.io/) was [declared as its successor](https://mailman.cs.umd.edu/pipermail/findbugs-discuss/2017-September/004383.html) in September 2017.

The current projects using `FindBugs` can make a shift to `SpotBugs` by following the [migration manual](http://spotbugs.readthedocs.io/en/latest/migration.html)

## Advanced Topics

- [Data mining of bugs with FindBugs](http://findbugs.sourceforge.net/manual/datamining.html): The data for each analysis will be collected and you can use these statistics for data mining.
- [Configure Analysis Properties](http://findbugs.sourceforge.net/manual/analysisprops.html#analysisproptable): You can define properties to configure options for some checks. For example, you can define the assertion methods in your project so that null pointer dereference bug detector will not raise violations if assertion methods are used.
- [Use annotations](http://findbugs.sourceforge.net/manual/annotations.html): You can use annotations to indicate your intents so that FindBugs can issue warnings more appropriately.
- [Writing custom detectors](https://www.ibm.com/developerworks/library/j-findbug2/): You can follow the tutorial step by step to write your customised detector.

## Resources

- [FindBugs Official Website](http://findbugs.sourceforge.net): Official website of FindBugs. You can find more documentations here.
- [An Evaluation of FindBugs](http://www.cs.cmu.edu/~aldrich/courses/654/tools/Sandcastle-FindBugs-2009.pdf): Analysis of FindBugs in 2009's version, some content may be outdated. Useful for understanding the benefits and drawbacks.
- [Improve the quality of your code](https://www.ibm.com/developerworks/library/j-findbug1/): Some examples showing the bugs reported by FindBugs. You can get a rough idea of how FindBugs will help you in your project.
