<frontmatter>
  title: Static Typing with TypeScript
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Static Typing with TypeScript

**Author(s): [Lu Yang Kenneth](https://github.com/luyangkenneth)**

Reviewer(s): WIP FILL THIS OUT LATER


## What is Static Typing?

**Statically typed languages** like Java, Go, and C++ are able to catch type-related errors at compile time, while in **dynamically typed languages** like JavaScript, such errors are not as easily discoverable because the types of variables are only known at runtime.

To help identify these problems without having to run any code, many developers have adopted technologies like Flow and TypeScript, which enable static type checking for codebases written in JavaScript.

Equivalent type checking tools exist for other dynamically typed languages like Python and Ruby as well, which suggests that this issue is not specific to the JavaScript community. The decision to move towards a statically typed codebase is usually motivated by the following benefits:

- Static type checking **reduces type-related errors during the development process**. These errors can be expensive to fix if they leak into production and cause downtime in your application, especially for teams that value stability over short-term productivity. A type checker provides an automatic way to verify the type safety and correctness of your application _before_ it goes live.

- Static type checking can **make developers more productive**. When paired with the appropriate IDE support, it enables features such as auto-completion and offers instant feedback by incrementally rechecking your code as you type.

- Static type checking **improves collaboration on a large codebase**. Explicitly declared types allow developers to refactor with greater confidence, knowing that API boundaries are enforced by the type checker. If you change a method signature or the shape of an object, you can quickly discover the other parts of the codebase that also need to be changed, as they would be surfaced as errors by the type checker.


### What's the difference between Flow and TypeScript?

**[Flow](https://flow.org/)** is a static type checker for JavaScript. It is an open-source tool developed by Facebook.

**[TypeScript](https://www.typescriptlang.org/)** is a statically typed superset of JavaScript that compiles to plain JavaScript. It is an open-source programming language developed by Microsoft.

While Flow and TypeScript integrate into your development workflow in slightly different ways, they have the same goals and share many similarities in terms of syntax. As the industry seems to be shifting towards TypeScript as the top choice <sup>[source](https://dev.to/nickytonline/is-2019-the-year-of-typescript-18p2)</sup>, in this article I will introduce how TypeScript works and how you can get started with TypeScript.


## TypeScript Basics

WIP


## Getting Started with TypeScript

WIP

</div>
