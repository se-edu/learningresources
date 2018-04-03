# Javascript: Promises

Author: Daniel Berzin Chua

## Why Promises?

We're used to operations completing their work before proceeding on with other operations in a sequential manner. These sorts of operations are synchronous, and they're usually easy to understand and debug. However, take an HTTP request for example. It is an operation that takes a while to process because there's a round trip time that involves the time for request to be sent to the server you are querying, and the time for the server to send the response back to you. It may take varying amounts of time depending on your internet speed and location.

If such an operation was to be executed in a synchronous manner, your application would be slow because it has to wait for this request to be resolved by the server and it would not make for a particularly good user experience. Instead, we can make HTTP requests to operate asynchronously in order to improve the speed and user experience of your program. Asynchronous operations do not wait for their work to be finished before proceeding on with other operations, which allow for them to continue processing in the background while other operations are executed. However, asynchronous operations can cause programming and debugging to be difficult, because you would need some sort of way to know when the operation has finished, or in the case of debugging, when exactly the operation is called. It can be especially confusing to trace the code since it may not run in the sequence that you would normally expect.

To illustrate this problem in code, we'll use `setTimeout()`. This is an asynchronous function takes in 3 parameters, a callback, a time (in milliseconds) to wait before the callback is executed and lastly, additional parameters to pass to the callback. A callback is a function that is passed as a parameter to another function, and it will be executed after that function finishes. `setTimeout()` is asynchronous as the code below it will execute while the timer is counting down, as you will see in the following code snippet.

You would probably expect `console.log(x)` to print `I have been updated` after 1 second has passed. Unfortunately, it prints `I have not been updated.` Give it a try in Google Chrome's developer console.

```javascript

var x = "I have not been updated";

setTimeout(function (){
    x = "I have been updated";
}, 1000);

console.log(x);

```

To fix this problem, we can simply shift the `console.log(x)` into the callback to get the expected result. It will now print the correct value.

```javascript

var x = "I have not been updated";

setTimeout(function (){
    x = "I have been updated";
    console.log(x);
}, 1000);

```

However, this fix will only go so far. If we had another `setTimeout()` that depended on the result of the earlier `setTimeout()`, we would have to nest the functions within each other which would make for unreadable code as follows. 

```javascript

var x = "I have not been updated";

setTimeout(function (){
    x = "I have been updated";
    console.log(x);
    setTimeout(function () {
        x = "I have been updated 2 times.";
        console.log(x);
        setTimeout(function () {
            x = "I have been updated 3 times.";
            console.log(x);
            // and so on...
        }, 1000);
    }, 1000);
}, 1000);

```
This is what's infamously known as callback hell. It's a natural result of using too many callbacks, as this would result in the code becoming deeply nested. It would be difficult for anyone to read your code and to understand what exactly is going on.

We can instead use Promises for cleaner code that would be easier to read, write and debug.

## What is a Promise?

The following example is adapted from [JavaScript Promises for Dummies](https://scotch.io/tutorials/javascript-promises-for-dummies).

Promises in Javascript behave the same way as Promises do in real life. Imagine that your friend promises to return you money that you have lent him. At the time this promise was made, you would not know if your friend would really return your money. Your friend could either return your money on time, or he could just not do it.
These situations correspond to the 3 states of Promises in Javascript.

| State | Description |
| ------ | ----------- |
|Pending | You don't know if he would return your money. |
|Fulfilled | He returned your money. |
|Rejected | He refused to return your money. |

Promises provide the ability to specify how the execution of some part of your code would depend on the status of an asynchronous operation. It can now wait for the asynchronous operation to resolve first before doing any work on its result.

## Quickstart

Let's return to the setTimeout example at the beginning and rewrite that with promises. 

```javascript

function update() {
    return new Promise((resolve, reject) => {
        var x = "I have not been updated";
        setTimeout(function() {
            x = "I have been updated";
            resolve(x);
        }, 1000);
    });
}

update().then(result => console.log(result));

```

The first step of creating a Promise involves invoking the `Promise` constructor which takes in just 1 parameter known as the executor function. This is the function that will be executed by the Promise. This function has two parameters: resolve, reject. In our example, we only use the resolve function to indicate that our promise has executed successfully with the result `x`. The reject function may be used to indicate that some error has occured during execution. 

After defining the function, we call it, and we call another function `.then()` upon the result. `.then()` is used to consume the result from a fulfilled Promise and act on it. In this case, we are using it to log our result. More importantly, it lets us chain promises to avoid the deep nesting known as callback hell.

Here's the callback hell code rewritten with Promises.

```javascript

function update(numTimes) {
    return new Promise((resolve, reject) => {
        var x = "I have not been updated";
        setTimeout(function() {
            x = "I have been updated " + numTimes + " times.";
            console.log(x);
            resolve(numTimes);
        }, 1000);
    });
}

update(1).then((result) => update(result + 1))
.then((result) => update(result + 1));

```

Now we update our function to take in a parameter `numTimes` so that we can log how many times `x` was updated. `then()` will return a Promise, which you can then again call `.then()`. By doing this, we can chain our Promises and ensure that execution of the code goes in the order that we require. In addition, it also allows your variables to be correctly passed down, ensuring that you will not have to deal with any undefined variables along the way.

Using Promises helps your code to be clean and easy to read, which makes it easy for anyone to instantly know what the code does, instead of having to trace through the code written with callbacks.

## How Promises are used

There are many interesting ways to use Promises. Here we cover HTTP requests in Javascript, and Disk I/O in Node.js. No prior knowledge of Node.js is necessary.

### HTTP Requests

Earlier in this chapter, HTTP requests were mentioned as an example of an asynchronous operation. By using Promises, you would be able to act on the result from the request without having to use callbacks or wait an arbitrarily set amount of time for the response to be returned.

The following code sends a GET request to a URL and logs the body of the response using Promises. By using Promises instead of callbacks, we have clean code and improved performance as the code is able to run in the background.

Code adapted from [Promise MDN docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

```javascript

function fetchPage(url) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.onload = () => resolve(xhr.responseText);
    xhr.onerror = () => reject(xhr.statusText);
    xhr.send();
  });
}

fetchPage('path/to/resource')
    .then(response => console.log(response))
    .catch(err => console.log(err));

```

### Disk I/O

Reading a file, especially a large one may take some time to complete. If we were to use a synchronous file reading function, the rest of your program wouldn't be able to run because it is stuck waiting for the file to be read. Instead, we can use asynchronous file reading functions which allow for background loading of the file, whilst keeping your program humming along.

```javascript

const fs = require('fs');   // this is the in-built filesystem module from Node.js

function readFileWithPromise(filePath) {
    return new Promise(function(resolve, reject) {
        fs.readFile(filePath, 'utf8', function(err, data) {
            if (err) {
                reject(err);
            } else {
                resolve(data);
            }
        });
    })

};

readFileWithPromise('path/to/file')
    .then(data => console.log(data))
    .catch(err => console.log(err));

```

## Other functions

Sometimes multiple promises may have to be used at a time, and Javascript provides excellent support with the `Promise.all` and `Promise.race` functions.

If multiple asynchronous operations have to be performed, and these operations are independent in that they do not rely on each other's values, `Promise.all()`can be used to execute all these operations at a go. It takes in an array of Promises and returns either an array with all the resolved values, or the value of the first rejected Promise. After which, `then()` which was previously mentioned, can be used to act on all these resolved values.

There is an excellent write up on these methods [here](https://davidwalsh.name/promises), which go through how best to use these functions.

In addition, there are other libraries such as [Bluebird](http://bluebirdjs.com/docs/getting-started.html) and [Q](https://github.com/kriskowal/q) which offer even more functionality such as Promise monitoring and synchronous inspection of Promises.

## Further Reading

You may read more about Promises, and how to use them at the following 2 websites:

[JavaScript Promises: an Introduction](https://developers.google.com/web/fundamentals/primers/promises)
[JavaScript Promises for Dummies](https://scotch.io/tutorials/javascript-promises-for-dummies)
