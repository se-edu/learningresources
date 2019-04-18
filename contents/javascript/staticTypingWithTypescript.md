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

### Compilation to JavaScript

As a superset of JavaScript, TypeScript is based on the same programming syntax as JavaScript, and valid JavaScript code is also valid TypeScript code. Besides static type checking, TypeScript offers support for the latest JavaScript features, including those from ECMAScript 2015 (ES6) and beyond.

While TypeScript can be configured to target different flavors of JavaScript, it is transpiled down to ES3 by default to maintain compatibility with all JavaScript engines. Because of this, you can use features like classes, generators, and async functions without worrying about whether your code is supported by a particular web browser or version of Node.js.


### Type Annotations and Type Inference

A key feature of TypeScript is the ability to add **type annotations** to your JavaScript variables and functions. This is useful for recording the intended contract of a variable or function.

Here's an example of declaring a variable with the `string` type and assigning it a `string` value:
```typescript
let myString: string = "abc";
```

If we try to assign it a `number` value later on, the TypeScript compiler will raise an error:
```typescript
myString = 123; // error: 123 is not assignable to type 'string'
```

The same type annotation syntax is used for function arguments and return types:
```typescript
function convertNumberToString(value: number): string {
    return value.toString();
}
```

TypeScript has the ability to perform **type inference**, where the compiler infers the types of your variables or functions automatically. This helps to cut down on excess verbosity in your code. In this next example, `myBoolean` is inferred as a `boolean` variable, so reassigning it to any `boolean` value is allowed:
```typescript
let myBoolean = true;

// valid
myBoolean = false;

// error: "hello world" is not assignable to type 'boolean'
myBoolean = "hello world";
```

Sometimes, we don't know what the type of a variable might be. In those cases, we would use the `any` type to opt out of compile-time type checking. For example, `myList` is an array that contains elements with a mix of different types:
```typescript
function printItemsToConsole(list: any[]): void {
    list.forEach(item => console.log(item));
}

const myList = [1, "hello", false];
printItemsToConsole(myList);
```

TypeScript also supports other variable types such as arrays and `void`, as seen in the example above. Check out [this article](https://www.typescriptlang.org/docs/handbook/basic-types.html) from the official TypeScript website for more details on those other types.


## Getting Started with TypeScript

WIP

</div>
