# FindBugs

Author: [Xiao Pu](https://nus-oss.github.io/cs3281-website/students/AY1617S2/xiaoPu/xiaoPu-Resume.html)

## Overview

FindBugs is a static analysis tool to find "bugs" in **Java** programme. It looks for "bug patterns" in the code and signals possible violations. For example, the bug "null pointer dereference" has the pattern — A programme declares a non-nullable variable but assigns `null` to the variable somewhere and uses it later.

## Features

The "bug patterns" can be divided into nine groups: Bad practice, Correctness, Experimental, Internationalization, Malicious code vulnerability, Multithreaded correctness, Performance, Security and Dodgy code. [A comprehensive list](http://findbugs.sourceforge.net/bugDescriptions.html) of bugs is provided to explain the meaning of each bug.

FindBugs analyses bytecode in compiled Java `.class` file and checks multiple files at the same time. These features overcome some limitations in [CheckStyle](CheckStyle.md) and [PMD](PMD.md) (The two tools can only check files one by one and analyse Java source code). Therefore, FindBugs can spot some errors that CheckStyle and PMD cannot find. For example, one of the bug patterns in FindBugs is `RCN: Redundant nullcheck of value known to be non-null`. FindBugs will analyse all the assignments to a particular variable in the code base and then check whether the `nullcheck` for the variable is redundant or not.

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

## Advanced Topic

- [Data mining of bugs with FindBugs](http://findbugs.sourceforge.net/manual/datamining.html): The data for each analysis will be collected and you can use these statistics for data mining.
- [Configure Analysis Properties](http://findbugs.sourceforge.net/manual/analysisprops.html#analysisproptable): You can define properties to configure options for some checks. For example, you can define the assertion methods in your project so that null pointer dereference bug detector will not raise violations if assertion methods are used.
- [Use annotations](http://findbugs.sourceforge.net/manual/annotations.html): You can use annotations to indicate your intents so that FindBugs can issue warnings more appropriately.
- [Writing custom detectors](https://www.ibm.com/developerworks/library/j-findbug2/): You can follow the tutorial step by step to write your customised detector.

## Resources

- [FindBugs Official Website](http://findbugs.sourceforge.net): Official website of FindBugs. You can find more documentations here.
- [An Evaluation of FindBugs](http://www.cs.cmu.edu/~aldrich/courses/654/tools/Sandcastle-FindBugs-2009.pdf): Analysis of FindBugs in 2009's version, some content may be outdated. Useful for understanding the benefits and drawbacks.
- [Improve the quality of your code](https://www.ibm.com/developerworks/library/j-findbug1/): Some examples showing the bugs reported by FindBugs. You can get a rough idea of how FindBugs will help you in your project.