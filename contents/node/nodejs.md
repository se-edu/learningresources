<frontmatter>
  title: Introduction to Node.js
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Introduction to Node.js

Author: Rachael Sim

# Overview

_This chapter assumes that the reader is familiar with JavaScript and asynchronous programming. If you are not familiar with asynchronous programming, a good resource to checkout is the [asynchronous programming section of the You Don't Know JS guide ](https://github.com/getify/You-Dont-Know-JS/blob/master/async%20&%20performance/README.md#you-dont-know-js-async--performance) as asynchronous programming is key in Node._

>*Node.js* is a JavaScript runtime built on Chrome’s V8 JavaScript engine. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. <sub>--https://nodejs.org</sub>

Node is mostly used in back-end and server side scenarios. For example, LinkedIn mobile app backend is built on Node JS and Uber built its massive matching system between customers and drivers on Node.js. However, Node can also be used in the front-end to automate tasks such as building, testing, pre and post processing code.

# Benefits

Why should you use Node?

## Benefit 1: Easy to get started

To install Node, simply download the installer from the [official Node.js website](https://nodejs.org/en/download/) based on your OS and run it. This should install both Node and npm. npm is a tool which will help you to search, install and manage node.js packages. The use of npm will be explained in the next section.

Here is an example of how to write a working Node.js server taken from [codeburst](https://codeburst.io/getting-started-with-node-js-a-beginners-guide-b03e25bca71b).

It is extremely short and simple but it demonstrates how a Node.js application import required modules, create a server to listen to client's request, read the request and returns a response.

Create a file `server.js` with following content:

```js
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer(function(req, res) {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
});
server.listen(port, hostname, function() {
  console.log('Server running at http://' + hostname + ':' + port + '/');
});
```

After you save the file, you can execute it from your terminal:
```
$ node server.js
Server running at http://127.0.0.1:3000/
```
To test the server, open a browser tab and navigate to http://localhost:3000/. You should see 'Hello world'.

Read on to find out about how to incorporate external dependencies, manage them and use Node's module system to organize your code.

## Benefit 2: Avoid synchronization problems and overheads

Node is designed to be **event-driven**. When an event happens e.g. an I/O event is complete, the event handler/callback function will be enqueued in a queue where it will be subsequently scheduled to run by the [event loop](https://medium.com/the-node-js-collection/what-you-should-know-to-really-understand-the-node-js-event-loop-and-its-metrics-c4907b19da4c). The main event loop (where Javascript code is executed) is single-threaded.

This means that we can avoid thread overheads and synchronization problems including deadlocks and race conditions.

## Benefit 3: Fast for I/O intensive tasks

This is due to the **non-blocking** I/O model.

When file and other I/O operations requests are made in Python or Java, they are blocking -- subsequent requests (in the same thread) have to be enqueued and can only be processed after the current operation and request completes. Meanwhile, the program remains idle. In contrast, Node.js has an event loop which delegates the actual tasks to other systems (e.g. file system and databases) and it is almost never blocked after delegation. While an I/O operation is incomplete, the event loop can still process subsequent requests.

## Benefit 4: Use JavaScript for both front and back-end development

Using Node for back-end development makes it possible to share and reuse code between the front-end and back-end. Teams can be more efficient and less reliant on documentation as they can read the code directly. The team can also become more cross-functional as developers can contribute to both development.

## Benefit 5: Easy dependency management with npm

Node Package Manager (npm) is used to
* Search for node.js packages online
* Install node.js packages from the command line, manage versions and dependencies effectively and easily

To load dependencies, anyone who want to use your project will just need to run the following command in the command shell.
```
$ npm install
```

This command will locate the `package.json` file (a file that contains all metadata information about a Node application) in the root directory and install all the dependencies specified in it.

A basic `package.json` has the following structure.
```json
{
  "name": "folder_name",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```
Every package needs a **name** and a **version**. Together, they should form a unique identifier. When a package is updated, the version number must be updated. A good **description** string and **keywords** e.g. `["promise", "lock"]` will help others to discover your package.

The `dependencies` property specifies dependencies needed in production while the `devDependencies` property specifies dependencies needed in development. It is also used to ensure consistency.

```json
{
  "dependencies": {
    "express": "~3.0.1",
    "sequelize":"latest",
    "bluebird": "^3.4.7",
    "angular":"latest",
  },
  "devDependencies": {
    "eslint": "^4.16.0",
    "eslint-config-airbnb-base": "^12.1.0",
    "eslint-plugin-import": "^2.8.0"
  }
}
```
The `dependencies` property maps to an object that has the name and version range for each dependency. It is common to find carets (`^`) and tildes (`~`) in the version range. To understand what they are for, checkout [NodeSource's article on semvers](https://nodesource.com/blog/semver-a-primer/). It is important to specify an appropriate version range to ensure consistency and so that users and developers will have compatible versions. For example, it is not advisable to always use the latest version of a dependency as it might introduce breaking changes (e.g. deprecate API) and the use of different versions by different developers and users might make debugging harder.

It is possible and easier to install a new dependency and update `package.json` directly from the command line with
```
$ npm install <package_name> --save
```
When a dependency is installed, the package's code will be added to the local `/node_modules` folder.

The [module section](#benefit-7-easy-to-reuse-code-from-others) describes how to import packages in your code.

## Benefit 6: npm `scripts` is versatile and help to save time in the development process

`package.json` also contains a `scripts` property. It is a useful tool that can be used to perform repetitive tasks such as testing and building. `scripts` takes in an object. For each property, the key is the name of the command and the corresponding value is the actual command that will be run.

For example, inside your `package.json`, you might have
```json
{
  "scripts": {
      "build": "node app.js",
      "lint": "eslint **/*.js",
      "lint-fix": "eslint --fix **/*.js"
  }
}
```
Running `npm run build` in the command shell will execute `node app.js` and similarly `npm run lint-fix` will fix your linting errors in your JavaScript files. npm scripts are much more powerful - you can use them as shortcuts to [uglify JavaScript, auto-prefix CSS](https://css-tricks.com/why-npm-scripts/) and even execute multiple tasks at a go. To find out more about how you can use npm scripts, refer to [freeCodeCamp's Introduction to NPM Scripts](https://medium.freecodecamp.org/introduction-to-npm-scripts-1dbb2ae01633).

## Benefit 7: Easy to reuse code from others

Node module system allows you to include other Javascript files and thus makes it easy to reuse external libraries and organize your code into separate parts with limited responsibilities.

Node comes bundled with useful [core modules](https://nodejs.org/api/) such as the `fs` (file system) module which includes classes, methods and events to deal with file I/O operations and the `http` module which helps Node to transfer data over HTTP.

There are also useful and well-tested non-core modules maintained by the community and external developers such as *Bluebird* for promises. Before importing them, remember to install them and include them in your `package.json` dependencies by following the instructions from the [npm section](#benefit-5-easy-dependency-management-with-npm).

Importing modules is simple - just use the `require()` function and provide the module identifier or the file path.
```js
const http = require('http'); // import a core module
const Promise = require('bluebird'); // import a non-core module
```

Node will first check if the module identifier passed to `require` is a core module or a relative path. If so, it will return the core module or the object which references the value of `module.exports` in the specified file path respectively. Otherwise, Node will attempt to load the module from the `node_modules` folder in the parent directory of the current module.

## Benefit 8: Good support

npm (Node Package Manager) is the largest ecosystem of open source libraries in the world. You can find a wide range of helpful and well-tested packages that can serve your needs on [npmjs](https://www.npmjs.com/).

## Benefit 9: Support for better code organization

We can also create separate files and modules in our codebase such that each module is focused on a single functionality. This makes your code more maintainable and testable.

To export the `Parser` constructor in `parser.js`,
```js
function Parser(options) {
    this._options = options || {};
}

Parser.prototype.parse = function (content) {
  // insert code here
}
module.exports = Parser; // override the exports object
```

The Parser constructor will then be returned whenever the `require` function is used to include the `Parser` module. It can be used to create Parser objects in other files in the project, for example,
```js
const parser = require('../parser') // import content from parser.js based on relative file path. The js extension is assumed and can be excluded.

const content = 'some content';
const newParser = new Parser();
newParser.parse(content);
```

To better understand how to use `module.exports`, checkout [Tendai Mutunhire's article](http://stackabuse.com/how-to-use-module-exports-in-node-js/).

# Use cases

Node.js is **good** for
* **Processing high volumes of IO-bound requests**. A single instance of a Node server will be more efficient and can serve more requests with the same hardware than other usual servers (such as those written in Ruby on Rails). This makes the Node server faster and more scalable.
* **Real time apps** where you have to process a large volume of requests with little delay. This includes instant messaging apps and collaborative editing apps where you can watch the document being modified live such as Trello and Google Docs. Node.js is a good choice as it can handle multiple client requests (even while waiting for responses).
* **Single-page apps** where a lot of processing and rendering is done on the client's side and the backend server only needs to provide a simple API. Node can process many requests with low response times. In addition, you can reuse and share code, such as those for validation, between the client and server.

However, Node.js is **not suitable** for
* **CPU-intensive jobs**. Remember that Node.js is single-threaded. If the thread is busy doing CPU-heavy operations, it will not be able to process incoming requests timely. This removes the biggest advantage of Node. Perhaps, you should consider an alternative technology such as Go which supports concurrency.


To find out more about when you should or should not use Node.js, checkout these articles by [netguru](https://www.netguru.co/blog/use-node-js-backend) and [Node.js foundation](https://medium.com/the-node-js-collection/why-the-hell-would-you-use-node-js-4b053b94ab8e).

# Resources
* Try out Node.js online - [Node prototyping with Runkit](https://runkit.com/home)
* A guide on asynchronous programming in JS - [You Don't Know JS: Async & Performance](https://github.com/getify/You-Dont-Know-JS/blob/master/async%20&%20performance/README.md#you-dont-know-js-async--performance)
* A comprehensive introduction to Node -  [The art of node](https://github.com/maxogden/art-of-node/)
* Making better use of npm scripts - [How to Use npm as a Build Tool](https://www.keithcirkel.co.uk/how-to-use-npm-as-a-build-tool/)
* Advice on how to write clean code that makes it easy to add new features - [Fundamental rules of a Node.js project structure](https://blog.risingstack.com/node-hero-node-js-project-structure-tutorial/)
* A compilation of useful node modules - [Awesome Nodejs](https://github.com/sindresorhus/awesome-nodejs)
*  A summary and curation of the top-ranked content on Node.js best practices - [Node.js Best Practices](https://github.com/i0natan/nodebestpractices)
* Node.js architecture and features - [All About Node.Js You Wanted To Know](https://codeburst.io/all-about-node-js-you-wanted-to-know-25f3374e0be7)
* More about `package.json` - [npm official documentation on package.json](https://docs.npmjs.com/files/package.json).

</div>
