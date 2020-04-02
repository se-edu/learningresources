<frontmatter>
  title: CheckStyle
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# CheckStyle

Author: [Xiao Pu](https://nus-oss.github.io/cs3281-website/students/AY1617S2/xiaoPu/xiaoPu-Resume.html)

<box id="article-toc">

* [Overview‎](#overview)
* [Features‎](#features)
* [Limitations‎](#limitations)
* [How to use it‎](#how-to-use-it)
  * [Configuration‎](#configuration)
  * [Suppress Warnings‎](#suppress-warnings)
  * [Running‎](#running)
* [Available Configurations‎](#available-configurations)
* [How does it work‎](#how-does-it-work)
* [Customisation‎](#customisation)
* [Resources‎](#resources)
</box>

## Overview

CheckStyle is a static analyser for **Java**. It can be used to assist developers in [static analysis](intro.html) process.

## Features
CheckStyle will examine code based on different rules(or checks).

According to the [checks list](http://checkstyle.sourceforge.net/checks.html) provided by CheckStyle, the checks(rules) can be divided into 14 sections.

- Annotations
- Block Checks
- Class Design
- Coding
- Headers
- Imports
- Javadoc Comments
- Metrics
- Miscellaneous
- Modifiers
- Naming Conventions
- Regexp
- Size Violations
- Whitespace

## Limitations
As described [here](http://checkstyle.sourceforge.net/writingchecks.html#Limitations), there are several limitations in CheckStyle.

- The code must be written in ASCII characters only.
- The examined code has to be compilable. The reason is described in [How does it work](#how-does-it-work) section.
- Files will be examined one by one, which means you cannot check multiple files at the same time.
	- For example, you cannot determine the full inheritance hierarchy of a class as you need to examine the parent class while checking the child class.

## How to Use It?

### Configuration
CheckStyle uses a [configuration file](http://checkstyle.sourceforge.net/config.html) to know all the rules that it is supposed to check.

### Suppress Warnings
CheckStyle supports suppressing warnings in four ways:

- [Annotations](http://checkstyle.sourceforge.net/config_filters.html#SuppressWarningsFilter)
- [Comments](http://checkstyle.sourceforge.net/config_filters.html#SuppressionCommentFilter)
- [File Filter](http://checkstyle.sourceforge.net/config_filefilters.html#BeforeExecutionExclusionFileFilter)
- [Configuration File](http://checkstyle.sourceforge.net/config_filters.html#SuppressionFilter)

### Running
There are several ways to run CheckStyle.

Command Line:

- [Command Line](http://checkstyle.sourceforge.net/cmdline.html)

Build Automation Tools:

- [Ant Task](http://checkstyle.sourceforge.net/anttask.html)
- [Maven Checkstyle](https://maven.apache.org/plugins/maven-checkstyle-plugin/)
- [Gradle Checkstyle](https://docs.gradle.org/current/userguide/checkstyle_plugin.html)

IDE Integration:

- [Eclipse Integration](http://eclipse-cs.sourceforge.net/#!/)
- [IntelliJ Integration](https://plugins.jetbrains.com/idea/plugin/1065-checkstyle-idea)

## Available Configurations
There are two widely used configurations: [Sun Code Conversions](http://www.oracle.com/technetwork/java/javase/documentation/codeconvtoc-136057.html) and [Google Java Style](http://checkstyle.sourceforge.net/reports/google-java-style.html). Some common rules are already included in these configurations.

## How Does It Work?
CheckStyle will use [ANTLR](http://www.antlr.org) to parse your code into a [AST(Abstract Syntax Tree)](https://en.wikipedia.org/wiki/Abstract_syntax_tree) and visit it in a [DFS(Depth First Search)](https://en.wikipedia.org/wiki/Depth-first_search) pattern to check violations. Thus, it is necessary to make the code compilable in order for the ANTLR to work.  You can view the syntax tree using [CheckStyle Grammar Tree Viewer](http://checkstyle.sourceforge.net/writingchecks.html#The_Checkstyle_SDK_Gui)

## Customisation
- [Writing Checks](http://checkstyle.sourceforge.net/writingchecks.html) (I want to write my own check for Java code.)
- [Writing Javadoc Checks](http://checkstyle.sourceforge.net/writingjavadocchecks.html) (I want to enforce new rules for writing Javadoc header comment.)
- [Writing Filters](http://checkstyle.sourceforge.net/writingfilters.html) (I will do something when violations are found.)
- [Writing File Filters](http://checkstyle.sourceforge.net/writingfilefilters.html) (I want to check the rules against specific files.)
- [Writing Listeners](http://checkstyle.sourceforge.net/writinglisteners.html) (I want different notifications (verbose printer, sending emails, etc) when violations are thrown.)

## Resources
- [CheckStyle](http://checkstyle.sourceforge.net/): CheckStyle official website. You can download the latest version, view online documentation there.
- [CheckStyle Github](https://github.com/checkstyle/checkstyle): CheckStyle GitHub page. You can contribute to the project or report bugs there.
- [StackOverflow CheckStyle](http://stackoverflow.com/questions/tagged/checkstyle): Question/Answer forum in StackOverflow for CheckStyle. You can ask question related to the using of CheckStyle.
</div>
