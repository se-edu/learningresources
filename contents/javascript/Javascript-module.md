<frontmatter>
  title: "Javascript: Module"
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Javascript: Module

Author: Gilbert Emerson

## What Is Module?

You might often encounter the term such as module, package, library, dependency, plugin, etc. We will not dive into the exact definition of each term (because that is not the scope of this chapter), but we will loosely define module as collection of code that is placed in different file for various reasons.

We would definitely want to modularize our code for good reason, which will be expanded on the section below.

## Why Use Module?

There can be a lot of reason why we would like to modularize our code, but here are 3 of the more important one:

1. Maintainability
A well designed module will be self-contained. Familiar principles of Software Engineering such as SOLID, etc can be applied to design a good module. A good module will allow a better maintanability of the module.

2. Namespacing
Because of how JavaScript scoping works (not convered in this article), namespacing can be quite a big issue in JavaScript because of global namespace pollution. Module allows JavaScript engineer to avoid this problem by isolating the namespace to a specific module.

3. Reusability
As per DRY Principle, module allows developers to reuse their code that is contained in a module.

## How Is It Implemented?

Historically, JavaScript did not support module system up until 2015 with the inception of EcmaScript6 (ES6). Nevertheless, there have been "unofficial implementations" of module sytem in JavaScript in form of Module Pattern and CommonJS.

This article will expand on those 3 implementations.

### Module Pattern

This is the way JavaScript developer create module in the past before the inception of CommonJS and ES6. Using a technique in JavaScript called IIFE (Immediately-Invoked Function Expression), they wrap their code in an IIFE.

> IIFE syntax </br>
> `(function() {})()`

`anExampleModule.js`
```javascript
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
```javascript
anExampleModule.getVariableOne(); // 1
anExampleModule.sumOfVariable(); // 3
```

`index.html`
```html
<script src="./anExampleModule.js" />
<script src="./index.js" />
```

In the above example, we are creating a module called `anExampleModule` in a separate `anExampleModule.js` file, which is used by `index.js` file.

To include the module in `index.js`, we can just use the `<script>` tag in HTML to import it.

### CommonJS

Implemented by NodeJS, it is one of the most popular "unofficial" module system implementation. It uses the syntax `module.export` and `require`.

`anExampleModule.js`
```Javascript
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
```Javascript
var anExampleModule = require('./anExampleModule.js');

anExampleModule.getVariableOne(); // 1
anExampleModule.sumOfVariable(); // 3
```

### EcmaScript2015 / ES6 module

At 2015, JavaScript introduced built-in module system in ES6. It uses the syntax `import` and `export`.

`anExampleModule.js`
```Javascript
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
```Javascript
import * as anExampleModule from './anExampleModule.js';

anExampleModule.getVariableOne(); // 1
anExampleModule.sumOfVariable(); // 3
```

### Which to Use?

While ES6 is the latest standard and is the official implementation from the JavaScript team, it is still not widely supported by all browsers. It requires the usage of transpiler and bundler tool such as Babel and Webpack respectively. CommonJS also faces this issue as well.

Apart from transpiling and bundling, both ES6 and CommonJS also has differences such as synchronous/asynchronous loading, support for tree shaking, static code analysis, etc. These differences will not be covered for now.

Nevertheless, both ES6 and CommonJS is a very valid approach in modularizing your JavaScript project. You can also get excuse to learn transpiling and bundling of JavaScript.

Despite the invention of ES6 and CommonJS, the old module pattern is also a valid approach in modularizing your code. It has the advantage of not requiring transpiling and bundling of your JavaScript code.

All in all, the best approach depends on your project requirements.

## How to Start?

You can start with module pattern right away by refactoring your code and separating it into separate files.

If you are planning to use CommonJS or ES6, you can start also by refactoring your code, but you will also need to be familiar with transpiler and bundler such as Babel and Webpack.

## Further Reading

You can read more on JavaScript module at following websites:

[JavaScript Modules: A Beginner’s Guide](https://medium.freecodecamp.org/javascript-modules-a-beginner-s-guide-783f7d7a5fcc) </br>
[JavaScript Modules: From IIFEs to CommonJS to ES6 Modules](https://tylermcginnis.com/javascript-modules-iifes-commonjs-esmodules/)