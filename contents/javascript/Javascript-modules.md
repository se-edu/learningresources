<frontmatter>
  title: "Javascript: Modules"
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Javascript: Modules

**Author: Gilbert Emerson**<br>
Reviewers: Chelsey Ong, Ong Shu Peng, Amrut Prabhu

## What Is a Module?

You might often encounter the term such as module, package, library, dependency, plugin, etc. when developing your own application in any programming languages, including JavaScript. We will not dive into the exact definition of each term (because that is not the scope of this chapter), but we will define module as a collection of code that share the common purpose or support a certain functionality that is placed in different file.

We would definitely want to modularize our code for good reason, which will be expanded on the section below.

## Why Use Modules?

**1. Better Maintainability** <br>
Having module will allow you to understand your code, test your code, and refactor your code easier, leading to better maintainability of your code. Without module, you will have one giant file that may be hard to navigate and understand, test, and refactor.

You might not only want to modularize your code, but also modularize it well. You can apply Software Engineering principles such as [SOLID](http://se-education.org/se-book/principles/solidPrinciples/index.html), etc to have good modularized code.

> **SOLID** is an acronym made up of the first letter of each of the following principles:
> 1. **S**ingle Responsibility Principle (SRP)
> 2. **O**pen-Closed Principle (OCP)
> 3. **L**iskov Substitution Principle (LSP)
> 4. **I**nterface Segregation Principle (ISP)
> 5. **D**ependency Inversion Principle (DIP)
>
> Click on the hyperlink above to find out more

**2. Namespacing** <br>
Because of how [JavaScript scoping](https://www.sitepoint.com/demystifying-javascript-variable-scope-hoisting/) works, avoiding global variable can be an issue. Modules allows JavaScript developer to avoid this problem by isolating the namespace to a specific module.

**3. Reusability** <br>
As per [Don't Repeat Yourself (DRY) Principle](http://se-education.org/se-book/principles/dryPrinciple/index.html), module allows developers to reuse their code that is contained in a module.

> **DRY (Don't Repeat Yourself) Principle** <br>
> Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.

## How Is It Implemented?

JavaScript did not support modules up until 2015 when ECMAScript6 (ES6) is rolled out. Nevertheless, there have been workarounds to implement modules in JavaScript in the form of Module Pattern and CommonJS.

This article will expand on those 3 implementations.

### Module Pattern

This is the way JavaScript developer create module in the past before the inception of CommonJS and ES6. Using a technique in JavaScript called [IIFE (Immediately Invoked Function Expression)](https://developer.mozilla.org/en-US/docs/Glossary/IIFE), they wrap their code in an IIFE.

> An **IIFE (Immediately Invoked Function Expression)** is a JavaScript function that runs as soon as it is defined. <br>
> Syntax: `(function() { statements })();`

In example below, we will create a module called `anExampleModule` in a separate `anExampleModule.js` file, which is used by `index.js` file.

To include the module in `index.js`, we use the `<script>` tag in HTML to import it.

```html
<!-- index.html -->

<script src="./anExampleModule.js" />
<script src="./index.js" />
```

```js
// anExampleModule.js

var anExampleModule = (function() {
    var variableOne = 1;
    var variableTwo = 2;

    var sumOfVariable = function() {
        return variableOne + variableTwo;
    }

    // We return an object with functions or variables
    // that we want to be exposed
    return {
        sumOfVariable: sumOfVariable
    }
})()
```

```js
// index.js

anExampleModule.sumOfVariable(); // 3
```

The result of importing the modules in the HTML will be as follow:
```js
// anExampleModule.js

var anExampleModule = (function() {
    var variableOne = 1;
    var variableTwo = 2;

    var sumOfVariable = function() {
        return variableOne + variableTwo;
    }

    // We return an object with functions or variables
    // that we want to be exposed
    return {
        sumOfVariable: sumOfVariable
    }
})()

// index.js

anExampleModule.sumOfVariable(); // 3
```

### CommonJS

It is one of the most popular modules implementation before ES6 is rolled out because it is widely used by NodeJS. It still can be used without using NodeJS by using [bundler](https://medium.com/@gimenete/how-javascript-bundlers-work-1fc0d0caf2da) such as [Webpack](https://webpack.js.org/).

Using CommonJS is quite similar to using IIFE, but instead of returning the functions or variable we want to expose, instead we assign it to `module.export`. To import the module, we can use `require` instead of using `<script>` tag in HTML.

See the following snippets for example of using CommonJS:

```js
// anExampleModule.js

var variableOne = 1;
var variableTwo = 2;

sumOfVariable = function() {
    return variableOne + variableTwo;
}

module.exports = {
  sumOfVariable: sumOfVariable,
};
```

```js
// index.js

var anExampleModule = require('./anExampleModule.js');

anExampleModule.sumOfVariable(); // 3
```

### EcmaScript2015 / ES6 modules

In 2015, JavaScript introduced built-in modules in ES6. It uses the syntax `import` and `export`.

```js
// anExampleModule.js

var variableOne = 1;
var variableTwo = 2;

export sumOfVariable() {
    return variableOne + variableTwo;
}
```

```js
// index.js

import * as anExampleModule from './anExampleModule.js';

anExampleModule.sumOfVariable(); // 3
```

### Which to use?

While ES6 modules is officially the most recent JavaScript standard rolled out, it is still not widely supported by all browsers. It needs [transpiler](https://scotch.io/tutorials/javascript-transpilers-what-they-are-why-we-need-them) such as [Babel](https://babeljs.io/) for unsupported browser. Similar to CommonJS, ES6 modules also requires bundler such as Webpack.

Despite ES6 modules not widely supported yet, it supports various advanced features not available in CommonJS such as asynchronous loading, support for [tree shaking](https://medium.com/@netxm/what-is-tree-shaking-de7c6be5cadd), static code analysis, etc. These features will not be covered in this article.

Nevertheless, both ES6 and CommonJS are a very valid approaches in modularizing your JavaScript project. You can take this opportunity to learn transpiling and bundling of JavaScript.

The invention of ES6 and CommonJS has not made the old module pattern obsolete. It is still a valid approach in modularizing your code. It has the advantage of not requiring transpiling and bundling of your JavaScript code.

All in all, the best approach depends on your project requirements. For example if your project does not allow you to use bundler or transpiler, then you will need to use IIFE. If your project will mostly be a NodeJS application, you will most likely be using CommonJS. If your project require you to ship minimal code as it is a high performing web application, ES6 will be better than CommonJS as it supports tree shaking.

## How to start?

You can start with module pattern right away by refactoring segments of your code into different files and wrapping the code in IIFE.

If you are planning to use CommonJS or ES6, you can start by refactoring segments of your code, but you will also need to be familiar with transpiler and bundler such as Babel and Webpack.

## Further Reading

You can read more on JavaScript modules at following websites:

- [JavaScript Modules: A Beginner’s Guide](https://medium.freecodecamp.org/javascript-modules-a-beginner-s-guide-783f7d7a5fcc) <br>
This article gives a broad explanation of JavaScript modules, explaining different kinds of module system in JavaScript, yet easy to follow.
- [JavaScript Modules: From IIFEs to CommonJS to ES6 Modules](https://tylermcginnis.com/javascript-modules-iifes-commonjs-esmodules/) <br>
This article truly focuses on IIFE, CommonJS, and ES6 Modules, giving a very comprehensive usage example.
- [Learn the basics of the JavaScript module system and build your own library](https://medium.freecodecamp.org/anatomy-of-js-module-systems-and-building-libraries-fadcd8dbd0e) <br>
While the aim of the article may not be the same with the aim of this chapter, this article gives a very comprehesive comparison between CommonJS and ES6 Modules.

Additional resources for related concepts (including all the hyperlinks above):

1. Software engineering principles <br>
[Software Engineering for Self-Directed Learners: SOLID Principles](http://se-education.org/se-book/principles/solidPrinciples/index.html) <br>
[Software Engineering for Self-Directed Learners: Don't Repeat Yourself (DRY)](http://se-education.org/se-book/principles/dryPrinciple/index.html)

2. JavaScript concepts <br>
[Demystifying JavaScript Variable Scope and Hoisting](https://www.sitepoint.com/demystifying-javascript-variable-scope-hoisting/) <br>
[IIFE - MDN Web Docs Glossary](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)

3. JavaScript development <br>
[How JavaScript bundlers work](https://medium.com/@gimenete/how-javascript-bundlers-work-1fc0d0caf2da) <br>
[JavaScript Transpilers: What They Are & Why We Need Them](https://scotch.io/tutorials/javascript-transpilers-what-they-are-why-we-need-them) <br>
[What is tree shaking? 🌲](https://medium.com/@netxm/what-is-tree-shaking-de7c6be5cadd)

4. Tools <br>
[Bundler: Webpack](https://webpack.js.org/) <br>
[Transpiler: Babel](https://babeljs.io/)