<frontmatter>
  title: Documentation
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Documentation

Authors: [Chua Ka Yi Ong](https://github.com/kychua)

<box id="article-toc">

* [Introduction‎](#introduction)
* [Why Write Documentation‎](#why-write-documentation)
* [Writing Good Documentation‎](#writing-good-documentation)
  * [Written Documentation‎](#written-documentation)
  * [Code Documentation‎](#code-documentation)
  * [Version Control‎](#version-control)
* [Resources‎](#resources)
* [Acknowledgements‎](#acknowledgements)
</box>

## Introduction

Software documentation refers to information that explains the code in a software project or how to use a software product or API. 
Many developers neglect writing documentation because they find it boring or troublesome, which is a pity because documentation communicates what you have done to the world - without documentation, people won't know about your awesome code or cool features.

> The difference between a tolerable programmer and a great programmer is not how many programming languages they know, and it’s not whether they prefer Python or Java. It’s whether they can communicate their ideas. By persuading other people, they get leverage. By writing clear comments and technical specs, they let other programmers understand their code, which means other programmers can use and work with their code instead of rewriting it. Absent this, their code is worthless. By writing clear technical documentation for end users, they allow people to figure out what their code is supposed to do, which is the only way those users can see the value in their code. There’s a lot of wonderful, useful code buried on sourceforge somewhere that nobody uses because it was created by programmers who don’t write very well (or don’t write at all), and so nobody knows what they’ve done and their brilliant code languishes.
>
> [Advice for Computer Science College Students](https://www.joelonsoftware.com/2005/01/02/advice-for-computer-science-college-students/), by Joel Spolsky

## Why Write Documentation

There are many benefits to writing good documentation, such as the following:

* **Makes things easier for yourself**: when you look back at your code a few months later, well-written documentation can make things a lot easier by explaining what you had been thinking when you were writing the code, when you find yourself confused by your own work
* **Improves your code**: documentation forces you to think carefully about what you are trying to do and why you have chosen to implement it in a particular way
* **Encourages others to contribute to your software**: by documenting how to contribute, it makes easier for others to do so, so they are more likely to help out
* **Encourages others to use your software**: ["If it isn't documented, it doesn't exist."](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=1680)

## Writing Good Documentation

Documentation should not be written for the sake of writing, but rather, to fulfill a need.
Good documentation is up-to-date and includes only what is necessary.

Documentation comes in many different forms.
Below are some guidelines on how to tackle each type of documentation.

### Written Documentation

Written documentation refers to what people usually think of as documentation - READMEs, user guides and developer guides.

In general, written documentation should use:

* grammatically correct language
* [simple language](https://wiki.zephyrproject.org/view/Documentation_style_guide#Simple_English): your documentation might be read by non-native speakers and use of idioms or flowery language can make it difficult for readers to understand
* markup: use formatting such as colors and italics and structural elements like admonition blocks to highlight important points
* realistic examples

Futhermore, you should always:

* [test your instructions](http://idratherbewriting.com/2015/07/07/testing-your-instructions/)
* find *someone else* to test your instructions
* [revise, revise and revise!](https://jacobian.org/writing/editors/)

[This](http://www.bluemangolearning.com/software-documentation/) provides useful general tips for writing documentation and 
[this guide](https://jacobian.org/writing/technical-style/) provides more tips on writing well. 

#### README

A README serves as an introduction to your project, and is often the first thing people see when encountering your project.<br>
A good README should convince users to use your product or contribute to your code, by providing enough information to make it easy for them to get started.

For a good guide on how to write a README, check out [this guide](http://www.writethedocs.org/guide/writing/beginners-guide-to-docs/).

* Some suggest that READMEs should be written before you start coding, as it gives you an overview of your project and what you need to do.
This is known as [Readme Driven Development](http://tom.preston-werner.com/2010/08/23/readme-driven-development.html).

#### User Guide

A user guide teaches users how to use your software.
User guides can include quickstart guides, reference materials, tutorials, cookbooks/recipes and more.

[Here](https://jacobian.org/writing/what-to-write/) is a useful guide on how to tackle different types of user documentation.
[This](http://stevelosh.com/blog/2013/09/teach-dont-tell/) points out the problems with common approaches to documentation and builds on the previous article.

#### Developer Guide

Developer guides teach new developers how to contribute to the code base.
They should give an overview of the code and an explanation of the design, as well as include instructions for setting up, making changes and deploying.

See [this](https://www.petrikainulainen.net/software-development/processes/writing-just-enough-documentation/) to learn more about what to include in a developer guide.

### Code Documentation

Code docummentation refers to documentation that is found within the source code.
All the following types of code documentation are important and complement one another.
Note that such documentation should always be updated together with the code.

#### Comments

Comments are a well-known form of documentation.
They [complement the code](http://softwareengineering.stackexchange.com/a/285789) and are intended for developers who use and work on the code.
They include API documentation for variables, methods, classes, and packages, as well as inline comments.

API documentation for variables, methods, classes, and packages explain what they do.
Good API documentation should enable users to use the API without having to guess how to use it or be confronted with unexpected behavior.
Thus, comments for methods should include the input and output, assumptions, edge cases and possible exceptions or errors thrown by the method.

Comments can also refer to inline comments.
Even the most well-written code can leave ambiguous questions. 
For example, code cannot explain why a line of code was written in a specific way, as discussed in [this article](https://blog.codinghorror.com/code-tells-you-how-comments-tell-you-why/). 
In such cases, well-written comments can clarify and make the code easier to understand. 

Note however, that excessive comments are also a code smell. 
Comments should not repeat what the code is saying and requiring a lot of comments to explain what you are doing can suggest that the code is too convoluted.
Often, comments can be replaced by clearer code.
As much as possible, code should be written as clearly as possible, with comments added in only when necessary.
[This Stack Overflow answer](http://stackoverflow.com/a/209089) gives a good example on how to do this.
For a more detailed explanation, read [this article](https://blog.codinghorror.com/coding-without-comments/).

#### Source Code

The source code is a form of documentation.
Well-written code makes it easy for developers to understand what the code is doing.
This can be done via a number of ways, such as

* using meaningful names for variables, functions and classes
* using useful error messages
* being consistent
* following software engineering principles

#### Tests

Tests are another form of documentation as they make the expected behavior of the methods clear for different types of input and edge cases.

### Version Control

Version control is also a form of documentation.
Having access to how the code base was changed over time can shows how, when and why a piece of code was written or changed. 
For this to work well, it is important to write good commit messages and follow good commit practices.

[This](https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message) explains what to include in commit messages while
[this article](http://mislav.net/2014/02/hidden-documentation/) explains how to use Git to make full use of your code's version control history.
For more resources on commit messages, check out [this Stack Overflow thread](https://stackoverflow.com/questions/15324900/standard-to-follow-when-writing-git-commit-messages).

## Resources

* For inspiration: https://github.com/PharkMillups/beautiful-docs
* An overview of the different types of documentation and good examples of each type: http://www.ybrikman.com/writing/2014/05/05/you-are-what-you-document/
* Documentation principles: http://www.writethedocs.org/guide/writing/docs-principles/
* Agile documentation: http://www.agilemodeling.com/essays/agileDocumentationBestPractices.htm
* Video on how to write great documentation: https://www.youtube.com/watch?v=z3fRu9pkuXE

## Acknowledgements

In addition to the links mentioned above, here are the links used in the writing of this article:

* https://opensource.com/business/15/5/write-better-docs
* http://www.developer.com/tech/article.php/10923_3848981_2/The-7-Rules-for-Writing-World-Class-Technical-Documentation.htm
* https://spin.atomicobject.com/2015/04/17/code-as-documentation/
* http://mikegrouchy.com/blog/2013/03/yes-your-code-does-need-comments.html
* https://chromium.googlesource.com/chromium/src/+/master/docs/documentation_best_practices.md#Documentation-is-the-story-of-your-code
* https://blog.jooq.org/2013/02/26/the-golden-rules-of-code-documentation/
* https://bryce.fisher-fleig.org/blog/effective-commit-messages/
* http://tom.preston-werner.com/2010/08/23/readme-driven-development.html
* http://www.cs.ecu.edu/karl/3300/spr14/Notes/Documentation/documentation.html
* http://www.tylerbutler.com/2013/06/the-importance-of-documentation/
* https://softwareengineering.stackexchange.com/questions/154615/are-unit-tests-really-used-as-documentation
* http://blog.smartbear.com/careers/13-things-people-hate-about-your-open-source-docs/
* https://byrslf.co/writing-great-documentation-44d90367115a#.qdxj6dcx4
* https://news.ycombinator.com/item?id=8414714
* http://www.developer.com/tech/article.php/10923_3848981_2/The-7-Rules-for-Writing-World-Class-Technical-Documentation.htm
* http://www.onlamp.com/pub/a/onlamp/2006/09/07/unit-testing-docs.html
* https://blog.jooq.org/2013/02/26/the-golden-rules-of-code-documentation/
* https://vip.wordpress.com/documentation/commit-messages/#a-good-commit-message-should-not-depend-on-the-code-to-explain-what-it-does-or-why-it-does-it
* https://news.ycombinator.com/item?id=10212582
* https://arialdomartini.wordpress.com/2012/09/03/pre-emptive-commit-comments/
* https://spin.atomicobject.com/2015/04/24/source-control-documentation/
* https://wiki.openstack.org/wiki/GitCommitMessages
* https://github.com/matiassingers/awesome-readme
</div>
