<frontmatter>
  title: Static Typing in JavaScript
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Static Typing in JavaScript

**Author(s): [Lu Yang Kenneth](https://github.com/luyangkenneth)**

Reviewer(s): [Marvin Chin](https://github.com/marvinchin)

<box id="article-toc">

* [Static Typing in JavaScript‎](#static-typing-in-javascript)
    * [What is Static Typing?‎](#what-is-static-typing)
    * [Adding Static Typing to JavaScript‎](#adding-static-typing-to-javascript)
    * [TypeScript Basics‎](#typescript-basics)
        * [Compilation to JavaScript‎](#compilation-to-javascript)
        * [Type Annotations and Type Inference‎](#type-annotations-and-type-inference)
        * [Custom Types‎](#custom-types)
        * [Optionals‎](#optionals)
    * [Getting Started with TypeScript‎](#getting-started-with-typescript)
        * [Installation‎](#installation)
        * [Additional Resources‎](#additional-resources)
</box>

## What is Static Typing?

Statically typed languages like Java, Go, and C++ are able to catch type-related errors at compile time. However, in dynamically typed languages like Python, Ruby, and JavaScript, such errors are not as easily discoverable because the types of variables are only known at runtime.

To help identify these problems without having to run any code, many developers have adopted technologies to enable static type checking for codebases written in dynamically typed languages. The decision to move towards a statically typed codebase is usually motivated by the following benefits:

- Static type checking **catches type errors earlier in the development cycle**. These errors can cause downtime in your application if undetected, and they are often expensive to triage and fix. Static type checking provides an automatic way to verify the type safety and correctness of your application during the development stage, ensuring that type errors are eliminated before your code is deployed to production. While the correctness of your application logic can also be determined via test suites and manual testing during development, those are often costlier and less reliable than static type checking when it comes to catching type errors.

- Static type checking can **make developers more productive**. With the additional type information made available to IDEs, features such as auto-completion, code hinting, incremental error checking, and automatic refactoring become more powerful.

- Static type checking **improves collaboration on a large codebase**. Statically typed code allows developers to refactor with greater confidence, knowing that API boundaries are enforced by the type checker. If you change the signature of a method, you can quickly discover the other parts of the codebase that need to be changed, as they would be surfaced as errors by the type checker. Explicitly declared types are also a form of documentation, which makes it easier to understand code written by other developers.


## Adding Static Typing to JavaScript

In the JavaScript community, Flow and TypeScript have emerged as the two main options for enabling static type checking:

- **[Flow](https://flow.org/)** is a static type checker for JavaScript. It is an open-source tool developed by Facebook.

- **[TypeScript](https://www.typescriptlang.org/)** is a statically typed superset of JavaScript that compiles to plain JavaScript. It is an open-source programming language developed by Microsoft.

While Flow and TypeScript integrate into your development workflow in slightly different ways, they have the same goals and share many similarities in terms of syntax. As the industry seems to be shifting towards TypeScript as the top choice <sup>[source](https://dev.to/nickytonline/is-2019-the-year-of-typescript-18p2)</sup>, this article will introduce how TypeScript works and how you can get started with TypeScript.


## TypeScript Basics

### Compilation to JavaScript

As a superset of JavaScript, TypeScript is based on the same programming syntax as JavaScript, and valid JavaScript code is also valid TypeScript code. TypeScript can be configured to target different flavors of JavaScript while offering support for the latest JavaScript features, including those from ECMAScript 2015 (ES6) and beyond.

Because TypeScript is <tooltip content="Transpilation is the process of translating source code into a different language or into a different flavor of the same language.">transpiled</tooltip> down to ES3 by default, it maintains compatibility with all JavaScript engines, and you can use features like classes, generators, and async functions without worrying about whether your code is supported by a particular web browser or version of Node.js.


### Type Annotations and Type Inference

TypeScript enables static type checking by giving you the ability to add **type annotations** to your JavaScript variables and functions. Here's an example of declaring a variable with the `string` type and assigning it a `string` value:
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

It isn't always necessary to add type annotations because TypeScript performs **type inference**, where the compiler infers the types of your variables or functions automatically. This helps to cut down on excess verbosity in your code. In this next example, `myBoolean` is inferred as a `boolean` variable, so reassigning it to any `boolean` value is allowed:
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


### Custom Types

In JavaScript, values are often passed around in the form of objects. Using TypeScript can help enforce the <tooltip content="The shape of an JavaScript object refers to its set of property keys.">shape</tooltip> of these objects. Let's use the `type` keyword to define our own `Person` type to describe an object that has the `firstName` and `lastName` properties:
```typescript
type Person = {
    firstName: string;
    lastName: string;
}
```

By using `Person` as a type annotation, TypeScript will now verify that any object we pass into the `greet` function has a compatible structure:
```typescript
function greet(person: Person) {
    console.log(`Nice to meet you, ${person.firstName} ${person.lastName}!`);
}

// valid
greet({ firstName: "John", lastName: "Doe" });

// error: property 'lastName' is required in type 'Person'
greet({ firstName: "John" });
```

This also helps to catch implementation mistakes such as:
```typescript
function askAboutColor(person: Person) {
    // error: property 'favoriteColor' does not exist on type 'Person'.
    console.log(`Hey ${person.firstName}, is it true that you like ${person.favoriteColor}?`);
}
```


### Optionals

You can specify optional object properties and function arguments, which are denoted by a `?` at the end of the property or argument name:

```typescript
type Student = {
    name: string;
    major?: string;
}
```

```typescript
function sayHello(name: string, favoriteColor?: string) {
    if (favoriteColor) {
        return `My name is ${name} and my favorite color is ${favoriteColor}!`;
    } else {
        return `My name is ${name}!`;
    }
}
```


## Getting Started with TypeScript

### Installation

The command-line TypeScript compiler can be installed as a Node.js package via `npm`:
```bash
npm install -g typescript
```

After installing TypeScript, you will have access to the `tsc` command for compiling TypeScript files:
```bash
tsc my_file.ts
```

This gets compiled into a `my_file.js` file, which you can execute in a browser or on Node.js.


### Additional Resources

- The [TypeScript Playground](https://www.typescriptlang.org/play/) is a convenient way to experiment with different scenarios in TypeScript (such as the examples in this article) without any setup required.

- [Visual Studio Code](https://code.visualstudio.com/) is probably the code editor with the best TypeScript support. This is unsurprising given that VS Code is another open-source project by Microsoft, and is itself written in TypeScript. That being said, TypeScript is widely supported by other code editors, either natively or via the appropriate package/extension.

- [TypeScript Deep Dive](https://basarat.gitbooks.io/typescript/) is the definitive online textbook on TypeScript which covers many practical topics.

- The official TypeScript guide on [Migrating from JavaScript](https://www.typescriptlang.org/docs/handbook/migrating-from-javascript.html) contains best practices for incrementally converting your codebase to TypeScript.

- For a more interactive demo of what it feels like to work with TypeScript, check out this YouTube video called [0-60 with TypeScript and Node.js](https://www.youtube.com/watch?v=vxvQPHFJDRo).

</div>
