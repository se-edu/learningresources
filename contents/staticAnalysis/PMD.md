<frontmatter>
  title: PMD
  footer: footer.md
  head: head.md
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# PMD

Author: [Xiao Pu](https://nus-oss.github.io/cs3281-website/students/AY1617S2/xiaoPu/xiaoPu-Resume.html)

## Overview

PMD is a static analyser for Java, JavaScript, Salesforce.com Apex, PL/SQL, Apache Velocity, XML, XSL. The copy/paste-detector([CPD](http://pmd.sourceforge.net/pmd-4.3.0/cpd.html)), which helps to find duplicated code, is also included as an add-on in PMD.

*Note that the links in this chapter is based on [PMD version 5.5.3](https://pmd.github.io/pmd-5.5.3/). You can check the latest version and documentation at its [GitHub Page](https://pmd.github.io/).*

## Features
Rules in PMD represent patterns in code. PMD is supposed to check these rules(patterns) and signal violations to programmers. For example, the `OverrideBothEqualsAndHashcode` rule requires programmers to override both `public boolean Object.equals(Object other)`, and `public int Object.hashCode()`, or override neither. 

PMD supports checking rules for the following languages.

- [Java](https://pmd.github.io/pmd-5.5.3/pmd-java/index.html) (includes [JSP](https://pmd.github.io/pmd-5.5.3/pmd-java/index.html) with [limitations](https://pmd.github.io/pmd-5.5.3/pmd-jsp/index.html))
- [JavaScript](https://pmd.github.io/pmd-5.5.3/pmd-javascript/index.html)
- [Apex](https://pmd.github.io/pmd-5.5.3/pmd-apex/index.html)
- [PL/SQL](https://pmd.github.io/pmd-5.5.3/pmd-plsql/index.html)
- [Velocity](https://pmd.github.io/pmd-5.4.1/pmd-vm/index.html)
- [XML and XSL](https://pmd.github.io/pmd-5.5.3/pmd-xml/index.html)

PMD doesn't support checking rules for the following languages. Only Copy/Paste Detector ([CPD](http://pmd.sourceforge.net/pmd-4.3.0/cpd.html)) is supported for them.

- [C++](https://pmd.github.io/pmd-5.5.3/pmd-cs/index.html)
- [C#](https://pmd.github.io/pmd-5.5.3/pmd-cpp/index.html)
- [Fortran](https://pmd.github.io/pmd-5.5.3/pmd-fortran/index.html)
- [Go](https://pmd.github.io/pmd-5.5.3/pmd-go/index.html)
- [Groovy](https://pmd.github.io/pmd-5.5.3/pmd-groovy/index.html)
- [Matlab](https://pmd.github.io/pmd-5.5.3/pmd-matlab/index.html)
- [Objective-C](https://pmd.github.io/pmd-5.5.3/pmd-objectivec/index.html)
- [Perl](https://pmd.github.io/pmd-5.5.3/pmd-perl/index.html) (only very basic CPD support)
- [PHP](https://pmd.github.io/pmd-5.5.3/pmd-php/index.html)
- [Python](https://pmd.github.io/pmd-5.5.3/pmd-python/index.html)
- [Ruby](https://pmd.github.io/pmd-5.5.3/pmd-ruby/index.html)
- [Swift](https://pmd.github.io/pmd-5.5.3/pmd-swift/index.html)
- [Scala](https://pmd.github.io/pmd-5.5.3/pmd-scala/index.html)

## Limitation
Limitations are almost the same as [CheckStyle](CheckStyle.md).

- The examined code has to be compilable. The reason is described in [How does it work](#how-does-it-work) section.
- Files will be examined one by one, which means you cannot check multiple files at the same time. 
	- For example, you cannot determine the full inheritance hierarchy of a class as you need to examine the parent class while checking the child class.

## How to use it

### Download
PMD can be run on both Windows and Linux/Unix operating system with the help of [Java JRE](http://www.oracle.com/technetwork/java/javase/overview/index.html) 1.7 or higher. Refer to [How to install PMD and CPD](https://pmd.github.io/pmd-5.5.3/usage/installing.html) for more details.

### Configuration
You can configure PMD to only include the rules that your want (see [How to make a new rule set](https://pmd.github.io/pmd-5.5.3/customizing/howtomakearuleset.html)).

### Suppress Warnings
PMD supports suppressing warnings in four ways:

- Annotations
- Comments
- Violation Suppress Regex
- Violation Suppress XPath

The details are described [here](https://pmd.github.io/pmd-5.5.3/usage/suppressing.html).

### Command Line Usage
PMD can be launched by using command line with various arguments. For details, please refer to [Running PMD via command line](https://pmd.github.io/pmd-5.5.3/usage/running.html).

### Integration with Build Automation Tools
- [Ant task usage](https://pmd.github.io/pmd-5.5.3/usage/ant-task.html)
- [The PMD Plugin in Gradle](https://docs.gradle.org/current/userguide/pmd_plugin.html)
- [Maven 1 PMD plugin](https://pmd.github.io/pmd-5.5.3/usage/maven-plugin.html)
- [Maven 2 PMD plugin](https://pmd.github.io/pmd-5.5.3/usage/mvn-plugin.html)

### Integration with IDEs
PMD can be integrated with most of IDEs, inlcuding BlueJ, CodeGuide, Eclipse, eclipse-pmd, Emacs, Gel, IntelliJ IDEA, IntelliJ IDEA - QAPlug, JBuilder, JCreator, JDeveloper, JEdit, Maven, Maven 2, NetBeans, TextPad, WebLOgic Workshop 8.1.x. For instruction to integrate with those IDEs, please refer to [PMD integrations](https://pmd.github.io/pmd-5.5.3/usage/integrations.html).

## Available Rulesets
There is not pre-defined rules set. You need to [define your rule sets](https://pmd.github.io/pmd-5.5.3/customizing/howtomakearuleset.html) by yourself.

PMD has organised rules into different categories. For example, the rules for Java has been categorised into 26 sections, which will help you quickly find the rules that you want.

You may refer to the [Features](#features) section to view the rules according to your languages and refer to [Configuration](#configuration) section to configure your rulesets.

## How does it work
PMD use [JavaCC](http://javacc.org) to parse your code to a [AST(Abstract Syntax Tree)](https://en.wikipedia.org/wiki/Abstract_syntax_tree) and visited it recursively ([more details](https://pmd.github.io/pmd-5.4.1/customizing/howitworks.html)). Thus, one of the requirements for PMD to work is that the code must be in valid Java syntax. You can view the syntax tree by using [bin/designer.bat](https://pmd.github.io/pmd-5.5.3/customizing/howtowritearule.html)

## Customization
- [How to write a PMD rule](https://pmd.github.io/pmd-5.5.3/customizing/howtowritearule.html) (A quicker way to write rule sets [Using XPath in PMD](https://pmd.github.io/pmd-5.4.1/customizing/xpathruletutorial.html), [XPath Rule tutorial](https://pmd.github.io/pmd-5.4.1/customizing/xpathruletutorial.html))
- [Add supports for new language](https://pmd.github.io/pmd-5.4.1/customizing/new-language.html) ([Add supports for CPD](https://pmd.github.io/pmd-5.4.1/customizing/cpd-parser-howto.html))

## Resources
- [PMD](https://pmd.github.io/): PMD official website. You can download the latest version, view online documentation there.
- [PMD Github](https://github.com/pmd/pmd): PMD GitHub page. You can contribute to the project or report bugs there.
- [StackOverflow PMD](https://stackoverflow.com/questions/tagged/pmd): Question/Answer forum in StackOverflow for PMD. You can ask question related to the using of PMD.
</div>
