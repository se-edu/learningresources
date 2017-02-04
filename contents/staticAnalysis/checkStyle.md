## CheckStyle
CheckStyle is a static analyser that assist developer in static analysis process.

### Features
According to the [checks list](http://checkstyle.sourceforge.net/checks.html), the checks can be divided into 14 sections.

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

### Limitation
As described in [here](http://checkstyle.sourceforge.net/writingchecks.html#Limitations), there are several limitation in CheckStyle.

- The code must be written in ASCII characters only.
- The examined code have to be compilable. The reason is describe in [How does it work section](how-does-it-work).
- File will be examined one by one, there is no check to check multiple file at the same time (e.g. you cannot determine the full inheritance hierarchy of a class)

### How to use it

#### Configuration
CheckStyle use a [configuration file](http://checkstyle.sourceforge.net/config.html) to know all the checks that it supposed to check.

#### Running
There are several way to run CheckStyle.

- [Ant Task](http://checkstyle.sourceforge.net/anttask.html)
- [Command Line](http://checkstyle.sourceforge.net/cmdline.html)
- [Eclipse Integration](http://eclipse-cs.sourceforge.net/#!/)
- [IntelliJ Integration](https://plugins.jetbrains.com/idea/plugin/1065-checkstyle-idea)

### Available Checks
There are two widely used configuration: [Sun Code Conversions](http://www.oracle.com/technetwork/java/javase/documentation/codeconvtoc-136057.html) and [Google Java Style](http://checkstyle.sourceforge.net/reports/google-java-style.html). They have already included common checks.

It is also possible to [customise](http://checkstyle.sourceforge.net/config.html) the configuration. [Here](http://checkstyle.sourceforge.net/checks.html) is the summary of all available checks. 

### How does it work
CheckStyle to parse your code to a [AST(Abstract Syntax Tree)](https://en.wikipedia.org/wiki/Abstract_syntax_tree) and visited it in a [DFS(Depth First Search)](https://en.wikipedia.org/wiki/Depth-first_search) patter to check violation. You can view the syntax tree using [CheckStyle Grammar tree Viewer](http://checkstyle.sourceforge.net/writingchecks.html#The_Checkstyle_SDK_Gui)

### Developer (Customisation)
- [Writing Checks](http://checkstyle.sourceforge.net/writingchecks.html)
- [Writing Javadoc Checks](http://checkstyle.sourceforge.net/writingjavadocchecks.html)
- [Writing Filters](http://checkstyle.sourceforge.net/writingfilters.html)
- [Writing File Filters](http://checkstyle.sourceforge.net/writingfilefilters.html)
- [Writing Listeners](http://checkstyle.sourceforge.net/writinglisteners.html)

### Reference
- [CheckStyle](http://checkstyle.sourceforge.net/)
- [CheckStyle Github](https://github.com/checkstyle/checkstyle)
- [StackOverflow CheckStyle](http://stackoverflow.com/questions/tagged/checkstyle)