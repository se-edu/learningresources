<frontmatter>
  title: "Javascript: Modules"
  pageNav: 3
</frontmatter>

<div class="website-content">

# Javascript: Modules

**Author: Gilbert Emerson**<br>
Reviewers: Chelsey Ong, Ong Shu Peng, Amrut Prabhu

<box id="article-toc">

* [What is a Module?‎](#what-is-a-module)
* [How to Modularize JavaScript Code?‎](#how-to-modularize-javascript-code)
    * [ES6 Modules‎](#es6-modules)
    * [CommonJS‎](#commonjs)
    * [Module Pattern‎](#module-pattern)
* [Which to Use?‎](#which-to-use)
* [How to Start?‎](#how-to-start)
* [Further Reading‎](#further-reading)
</box>

<box type="info">

This article assumes the reader has some basic knowledge of JavaScript.
</box>

## What is a Module?

In programming, the term module (other similar terms: _package, library, dependency, plugin_) is used to refer to _a small part of code that is broken up from a larger code base_.

Modules help programmers in many ways. Here are some of the examples:

**1. Modules make code managable** <br>
Using modules to break a code base into smaller parts can make it more manageable, especially for a large code base.

Let's say you have an application with functionalities A and B, where functionality A needs functionality B. Without modules, both of these functionalities are mixed together in the code base without a clear separation. With modules, we can separate these 2 functionalities. When A needs B, A will "include" B and A will be able to work as if A and B had never been separated.

**2. Modules help minimize name clashes** <br>
Breaking code into modules results in breaking the code's namespace into smaller parts too. This will help in minimizing name clashes and the need for global variables.

**3. Modules promote reuse** <br>
Modules allow developers to reuse their code. If we have an application that relies on the same string comparison function in multiple files, we can separate that function into a module. Now we can "include" the function from that module instead of repeating that function definition in all the files where it is needed.

## How to Modularize JavaScript Code?

There are 3 common ways to use modules in JavaScript: 1. using ES6 modules, 2. using CommonJS, 3. using the module pattern. While ES6 modules is the most recent and the official implementation, this article covers the other two as well because there are still a large number of existing projects that use them.

### ES6 Modules

Introduced in 2015, ES6 modules is the official implementation of modules in JavaScript. It introduces 2 new syntax `import` and `export` to support modules in JavaScript.

In the example below, `index.js` needs the function `sumOfVariable` from `anExampleModule.js`. So, `anExampleModule.js` will need to _export_ the function using the `export` syntax and `index.js` will need to _import_ that function using the `import` syntax.

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

ES6 modules supports advanced features such as _[asynchronous loading](https://exploringjs.com/es6/ch_modules.html#sec_modules-in-browsers), [tree shaking](https://medium.com/@netxm/what-is-tree-shaking-de7c6be5cadd), [static code analysis](https://exploringjs.com/es6/ch_modules.html#static-module-structure)_ and [dynamic imports](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#Dynamic_Imports). These features will not be covered in this article.

A more in-depth explanation of ES6 modules can be found in the [Modules chapter of the Exploring ES6 online book](https://exploringjs.com/es6/ch_modules.html).

ES6 modules is supported by all major browsers as of 2020. However, you may need to support browsers that do not implement ES6 modules.

One possible solution is to use a _[transpiler](https://scotch.io/tutorials/javascript-transpilers-what-they-are-why-we-need-them)_ such as _[Babel](https://babeljs.io/)_ and a _[bundler](https://medium.com/@gimenete/how-javascript-bundlers-work-1fc0d0caf2da)_ such as _[Webpack](https://webpack.js.org/)_ to serve your application to unsupported browsers.

Alternatively, use the two other methods described below.

### CommonJS

CommonJS is in wide use today because it is used by _NodeJS_ which in turn is used by many JavaScript applications.

The example below will replicate the same example in the previous section using CommonJS. `index.js` needs the function `sumOfVariable` from `anExampleModule.js`. So, `anExampleModule.js` module will need to _export_ the function using the `module.exports` syntax and `index.js` module will need to _import_ that function using the `require` syntax.

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

A more in-depth explanation of CommonJS can be found in the [Modules chapter of NodeJS API documentation](https://nodejs.org/docs/latest/api/modules.html).

Note that CommonJS modules only work natively in NodeJS. If you would like to use CommonJS modules in browsers, you will still need to use a bundler such as _[Webpack](https://webpack.js.org/)_.

If you are only writing JavaScript for the client side and not using NodeJS at all, and also need to support a wide variety of browsers, consider using the last method described. It has been supported by browsers ever since they started supporting JavaScript.

### Module Pattern via Namespaces

Using a technique in JavaScript called _[IIFE (Immediately Invoked Function Expression)](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)_, JavaScript developers can create a namespace by wrapping their code in an IIFE.

<box>

An IIFE (Immediately Invoked Function Expression) is a JavaScript function that runs as soon as it is defined. <br>
**Syntax:** `(function() { statements })();`

_Source: [MDN Glossary - IIFE](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)_
</box>

The example below will replicate the same example in previous sections using the module pattern. `index.js` needs the function `sumOfVariable` from `anExampleNamespace.js`. To achieve this, we use the `<script>` tag in HTML to include `anExampleNamespace.js` for `index.js` to use.

```html
<!-- index.html -->

<script src="./anExampleNamespace.js" />
<script src="./index.js" />
```

```js
// anExampleNamespace.js

var anExampleNamespace = (function() {
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

anExampleNamespace.sumOfVariable(); // 3
```

The result of including the script containing the `anExampleNamespace` object in the HTML will be as follows:
```js
// anExampleNamespace.js

var anExampleNamespace = (function() {
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

anExampleNamespace.sumOfVariable(); // 3
```

A more in-depth explanation of the module pattern can be found in the [this course blog on mastering module pattern](https://ultimatecourses.com/blog/mastering-the-module-pattern).

## Which to Use?

Although ES6 modules is the official way to implement modules, there are situations where you might have to use one of the other options. Here are some examples:
- If your application does not allow you to use a transpiler or a bundler (e.g. because of the additional overhead they add), you can use the module pattern via namespaces.
- If your application is using Node.js, you might want to use CommonJS because Node.js only has experimental support for ES6 modules as of Node.js 13.8.0.

## How to Start?

You can use the module pattern right away by refactoring segments of your code into different files and wrapping your code in an IIFE.

If you are planning to use CommonJS or ES6 modules, you can start by refactoring segments of your code, but you will also need to be familiar with transpilers and bundlers such as Babel and Webpack.

## Further Reading

You can read more on JavaScript modules at following websites:

- [JavaScript Modules: A Beginner’s Guide](https://medium.freecodecamp.org/javascript-modules-a-beginner-s-guide-783f7d7a5fcc) <br>
This article gives a broad explanation of JavaScript modules, explaining different kinds of module system in JavaScript, yet easy to follow.
- [JavaScript Modules: From IIFEs to CommonJS to ES6 Modules](https://tylermcginnis.com/javascript-modules-iifes-commonjs-esmodules/) <br>
This article truly focuses on IIFE, CommonJS, and ES6 Modules, giving a very comprehensive usage example.
- [Learn the basics of the JavaScript module system and build your own library](https://medium.freecodecamp.org/anatomy-of-js-module-systems-and-building-libraries-fadcd8dbd0e) <br>
While the aim of the article may not be the same with the aim of this book article, this article gives a very comprehesive comparison between CommonJS and ES6 Modules.
- [Namespacing in Javascript](https://javascriptweblog.wordpress.com/2010/12/07/namespacing-in-javascript/)
