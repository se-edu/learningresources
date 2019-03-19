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

Author: Gilbert Emerson

## What Is a Module?

You might often encounter the term such as module, package, library, dependency, plugin, etc. We will not dive into the exact definition of each term (because that is not the scope of this chapter), but we will loosely define module as a collection of code that is placed in different file for various reasons.

We would definitely want to modularize our code for good reason, which will be expanded on the section below.

## Why Use Modules?

There can be a lot of reasons why we would like to modularize our code, but here are 3 of the more important one:

1. Maintainability <br>
A well designed module will be self-contained. Familiar principles of Software Engineering such as SOLID <sup>[link](http://se-education.org/se-book/principles/solidPrinciples/index.html)</sup>, etc can be applied to design a good module. A good module will allow a better maintanability of the module.

2. Namespacing <br>
Because of how JavaScript scoping works (not covered in this article), avoiding global variable can be an issue. Modules allows JavaScript developer to avoid this problem by isolating the namespace to a specific module.

3. Reusability <br>
As per Don't Repeat Yourself (DRY) Principle <sup>[link](http://se-education.org/se-book/principles/dryPrinciple/index.html)</sup>, module allows developers to reuse their code that is contained in a module.

## How Is It Implemented?

Historically, JavaScript did not support modules up until 2015 with the inception of EcmaScript6 (ES6). Nevertheless, there have been "unofficial implementations" of modules in JavaScript in the form of Module Pattern and CommonJS.

This article will expand on those 3 implementations.

### Module Pattern

This is the way JavaScript developer create module in the past before the inception of CommonJS and ES6. Using a technique in JavaScript called IIFE (Immediately-Invoked Function Expression), they wrap their code in an IIFE.

> IIFE syntax <br>
> `(function() {})()`

`index.html`
```html
<script src="./anExampleModule.js" />
<script src="./index.js" />
```

`anExampleModule.js`
```js
var anExampleModule = (function() {
    var variableOne = 1;
    var variableTwo = 2;

    var getVariableOne = function() {
        return variableOne;
    }

    var sumOfVariable = function() {
        return variableOne + variableTwo;
    }

    var counter = function() {
        variableOne++;
    }

    return {
        getVariableOne: getVariableOne,
        sumOfVariable: sumOfVariable,
        counter,
    }
})()
```

`index.js`
```js
anExampleModule.getVariableOne(); // 1
anExampleModule.sumOfVariable(); // 3
```

In the above example, we are creating a module called `anExampleModule` in a separate `anExampleModule.js` file, which is used by `index.js` file.

To include the module in `index.js`, we use the `<script>` tag in HTML to import it.

The result of importing the modules in the HTML will be as follow:
```js
// anExampleModule.js
var anExampleModule = (function() {
    var variableOne = 1;
    var variableTwo = 2;

    var getVariableOne = function() {
        return variableOne;
    }

    var sumOfVariable = function() {
        return variableOne + variableTwo;
    }

    var counter = function() {
        variableOne++;
    }

    return {
        getVariableOne: getVariableOne,
        sumOfVariable: sumOfVariable,
        counter,
    }
})()

// index.js
anExampleModule.getVariableOne(); // 1
anExampleModule.sumOfVariable(); // 3
```

### CommonJS

It is one of the most popular "unofficial" modules implementation because it is widely used by NodeJS. Nevertheless, it can be used without using NodeJS by using bundler <sup>[link](https://medium.com/@gimenete/how-javascript-bundlers-work-1fc0d0caf2da)</sup> such as Webpack <sup>[link](https://webpack.js.org/)</sup>.

It uses the syntax `module.export` and `require`.

`anExampleModule.js`
```js
var variableOne = 1;
var variableTwo = 2;

getVariableOne = function() {
  return variableOne;
}

sumOfVariable = function() {
    return variableOne + variableTwo;
}

module.exports = {
  getVariableOne: getVariableOne,
  sumOfVariable: sumOfVariable,
};
```

`index.js`
```js
var anExampleModule = require('./anExampleModule.js');

anExampleModule.getVariableOne(); // 1
anExampleModule.sumOfVariable(); // 3
```

### EcmaScript2015 / ES6 modules

In 2015, JavaScript introduced built-in modules in ES6. It uses the syntax `import` and `export`.

`anExampleModule.js`
```js
var variableOne = 1;
var variableTwo = 2;

export getVariableOne() {
    return variableOne;
}

export sumOfVariable() {
    return variableOne + variableTwo;
}
```

`index.js`
```js
import * as anExampleModule from './anExampleModule.js';

anExampleModule.getVariableOne(); // 1
anExampleModule.sumOfVariable(); // 3
```

### Which to use?

While ES6 modules is officially the most recent JavaScript standard rolled out, it is still not widely supported by all browsers. It needs transpiler <sup>[link](https://scotch.io/tutorials/javascript-transpilers-what-they-are-why-we-need-them)</sup> such as Babel <sup>[link](https://babeljs.io/)</sup> for unsupported browser. Similar to CommonJS, ES6 modules also requires bundler such as Webpack.

Despite ES6 modules not widely supported yet, it supports various advanced features not available in CommonJS such as asynchronous loading, support for tree shaking, static code analysis, etc. These features will not be covered for now.

Nevertheless, both ES6 and CommonJS are a very valid approaches in modularizing your JavaScript project. You can take this opportunity to learn transpiling and bundling of JavaScript.

The invention of ES6 and CommonJS has not made the old module pattern obsolete. It is still a valid approach in modularizing your code. It has the advantage of not requiring transpiling and bundling of your JavaScript code.

All in all, the best approach depends on your project requirements.

## How to start?

You can start with module pattern right away by refactoring segments of your code into different files and wrapping the code in IIFE.

If you are planning to use CommonJS or ES6, you can start by refactoring segments of your code, but you will also need to be familiar with transpiler and bundler such as Babel and Webpack.

## Further Reading

You can read more on JavaScript modules at following websites:

[JavaScript Modules: A Beginner’s Guide](https://medium.freecodecamp.org/javascript-modules-a-beginner-s-guide-783f7d7a5fcc) <br>
[JavaScript Modules: From IIFEs to CommonJS to ES6 Modules](https://tylermcginnis.com/javascript-modules-iifes-commonjs-esmodules/)