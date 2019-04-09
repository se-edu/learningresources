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

## Preface

This article will be covering on JavaScript modules: What is it, Why use it, and How to use it. While there will be links to various external resources explaining terms that is not covered by this article, readers are adviced to be familiar with JavaScript.

## What Is a Module?

You might often encounter the term such as *module, package, library, dependency, plugin,* etc. when developing your own application in any programming languages, including JavaScript. We will not dive into the exact definition of each term (because that is not the scope of this article), but we will define a module as a collection of code that share the common purpose or support a certain functionality that is placed in different file.

Let's say you have an application with functionalities A and B, where functionality A needs functionality B. Without modules, both of these functionalities are mixed together in the code base without a clear separation. With modules, we can separate those 2 functionalities into separate module each. When A need B, A will "include" B and A will be able to work as if A and B has never been separated. The exact working of modules will be illustrated in the **How to Modularize JavaScript Code** section after the following section.

## Why Use Modules in JavaScript?

Following are reasons why use modules in JavaScript. The reasons below are not exclusive to JavaScript, but may also be general reasons for using modules in other languages.

**1. Better management of code base** <br>
Using modules will break the code base into smaller parts which if done well can help in managing a code base, especially a large one. To be able to modularize your code well, you can apply Software Engineering principles such as [SOLID](http://se-education.org/se-book/principles/solidPrinciples/index.html), etc.

**2. Namespacing** <br>
Because of how [JavaScript scoping](https://www.sitepoint.com/demystifying-javascript-variable-scope-hoisting/) works, avoiding global variable can be an issue. Modules allows JavaScript developer to avoid this problem by isolating the namespace to a specific module.

**3. Reusability** <br>
As per [Don't Repeat Yourself (DRY) Principle](http://se-education.org/se-book/principles/dryPrinciple/index.html), modules allows developers to reuse their code that is contained in a module. If for example we have an application that rely on a fuctionality such as string comparison function, we can separate that function into a module and let the application to use the function from that module instead of having to always repeating that function in all the places where it is needed.

## How to Modularize JavaScript Code?

There are 3 most common implementations of modules in JavaScript: ES6 modules as the most recent and official implementation in JavaScript, CommonJS which is used by NodeJS, and the old module pattern. This article will cover all those 3 implementations because there is still a large number of applications using those 3 implementations.

### ES6 modules

Introduced in 2015, ES6 modules is the official implementation of modules in JavaScript. It introduces 2 new syntax `import` and `export` to use modules in JavaScript.

In the example below, `index.js` needs the function `sumOfVariable` from `anExampleModule.js`. So, `anExampleModule.js` module will need to "export" the function using `export` syntax and `index.js` module will need to "import" that function using `import` syntax.

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

Due to the very recent adoption of ES6 modules by browsers and there is still some browsers that do not support it, you might not be able to use ES6 modules right away in those unsupported browsers. Fear not there are 2 workarounds for this issue. You can use [transpiler](https://scotch.io/tutorials/javascript-transpilers-what-they-are-why-we-need-them) such as [Babel](https://babeljs.io/) and [bundler](https://medium.com/@gimenete/how-javascript-bundlers-work-1fc0d0caf2da) such as [Webpack](https://webpack.js.org/) to serve your application to those unsupported browsers, or use other implementations of modules which will be given below.

### CommonJS

It is one of the most popular modules implementation before ES6 module is rolled out because it is widely used by NodeJS. While it can be only be used outside of NodeJS by also using bundler like ES6 module, it doesn't need transpiler. Worry not if you also cannot use bundler because there is still another module implementation that you can use. Nevertheless CommonJS is still a valid module implementation because it is still used very extensively in NodeJS applications.

Using CommonJS is very similar to using ES6 modules because everything else is the same, except the syntax. CommonJS uses `module.exports` and `require` syntax.

The following example are the same with the one in ES6 modules, but implemented with CommonJS syntax:

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

### Module Pattern

This is the way JavaScript developers implement modules in the past before the inception of CommonJS and ES6 modules. Using a technique in JavaScript called [IIFE (Immediately Invoked Function Expression)](https://developer.mozilla.org/en-US/docs/Glossary/IIFE), they wrap their code in an IIFE.

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

### Which to use?

Despite ES6 modules being not supported by some browsers yet, it supports various advanced features not available in CommonJS such as asynchronous loading, support for [tree shaking](https://medium.com/@netxm/what-is-tree-shaking-de7c6be5cadd), static code analysis, etc. These features will not be covered in this article.

Nevertheless, both ES6 modules and CommonJS are a very valid approaches in modularizing your JavaScript project. You can take this opportunity to learn transpiling and bundling of JavaScript.

The invention of ES6 modules and CommonJS has not made the old module pattern obsolete. It has the advantage of not requiring transpiling and bundling of your JavaScript code.

All in all, the best approach depends on your project requirements. For example if your project does not allow you to use bundler or transpiler, then you will need to use IIFE. If your project will mostly be a NodeJS application, you will most likely be using CommonJS. If your project require you to ship minimal code as it is a high performing web application, ES6 modules will be better than CommonJS as it supports tree shaking.

## How to start?

You can start with module pattern right away by refactoring segments of your code into different files and wrapping the code in IIFE.

If you are planning to use CommonJS or ES6 modules, you can start by refactoring segments of your code, but you will also need to be familiar with transpiler and bundler such as Babel and Webpack.

## Further Reading

You can read more on JavaScript modules at following websites:

- [JavaScript Modules: A Beginner’s Guide](https://medium.freecodecamp.org/javascript-modules-a-beginner-s-guide-783f7d7a5fcc) <br>
This article gives a broad explanation of JavaScript modules, explaining different kinds of module system in JavaScript, yet easy to follow.
- [JavaScript Modules: From IIFEs to CommonJS to ES6 Modules](https://tylermcginnis.com/javascript-modules-iifes-commonjs-esmodules/) <br>
This article truly focuses on IIFE, CommonJS, and ES6 Modules, giving a very comprehensive usage example.
- [Learn the basics of the JavaScript module system and build your own library](https://medium.freecodecamp.org/anatomy-of-js-module-systems-and-building-libraries-fadcd8dbd0e) <br>
While the aim of the article may not be the same with the aim of this book article, this article gives a very comprehesive comparison between CommonJS and ES6 Modules.

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