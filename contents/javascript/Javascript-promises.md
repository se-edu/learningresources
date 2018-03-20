# Javascript: Promises

Author: Daniel Berzin Chua

## Why Promises?

We're used to operations completing their work before proceeding on with other operations in a sequential manner. These sorts of operations are synchronous, and they're usually easy to understand and debug. However, take a HTTP request for example. It is an operation that takes a while to process because there's a round trip time that involves the time for request to be sent to the server you are querying, and the time for the server to send the response back to you. It may take varying amounts of time depending on your internet speed and location.

If such an operation was to be executed in a synchronous manner, your application would be slow because it has to wait for this request to be resolved by the server and it would not make for a particularly good user experience. Instead, we can make HTTP requests to operate asynchronously in order to improve the speed and user experience of your program. Asynchronous operations do not wait for their work to be finished before proceeding on with other operations, which allow for them to continue processing in the background while other operations are executed. However, asynchronous operations can cause programming and debugging to be difficult, because you would need some sort of way to know when the operation has finished, or in the case of debugging, when exactly the operation is called. It can be especially confusing to trace the code since it may not run in the sequence that you would normally expect.

To illustrate this problem in code, we'll use `readFile()` from the `fs` package in Node.js as an example. This is an asynchronous function takes in 3 parameters, a callback and a time (in milliseconds) to wait before the callback is executed. A callback is a function that is passed as a parameter to another function (call this A), and it will be executed after A finishes.

In the following code snippet, you would probably expect `console.log(x)` to print `I have been updated`. Unfortunately, it prints `I have not been updated.` Give it a try in Google Chrome's developer console.

```javascript

var x = "I have not been updated";

setTimeout(function (){
    x = "I have been updated";
}, 1000);

console.log(x);

```

To fix this, we can simply shift the `console.log(x)` into the callback to get the expected result. It will now print the correct value.

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
    setTimeout(function () {
        x = "I have been updated 2 times.";
        setTimeout(function () {
            x = "I have been updated 3 times.";
            // and so on...
        }, 1000);
    }, 1000);
}, 1000);

```

That's pretty ugly and it would be difficult for anyone to read your code and to understand what exactly is going on.

We can instead use Promises for cleaner code that would be easier to write and debug.

## What is a Promise?

The following example is adapted from [JavaScript Promises for Dummies](https://scotch.io/tutorials/javascript-promises-for-dummies).

Promises in Javascript behave the same way as Promises do in real life. Imagine that your friend promises to return you money that you have lent him. At the time this promise was made, you would not know if your friend would really return your money. Your friend could either return your money on time, or he could just not do it.
These situations correspond to the 3 states of Promises in Javascript.

| State | Description |
| ------ | ----------- |
|Pending | You don't know if he would return your money. |
|Fulfilled | He returned your money. |
|Rejected | He refused to return your money. |


Promises provide the power to control the execution flow of your program by allowing you to wait for a asynchronous operation to complete before carrying on with the rest of the program execution.

## How Promises are used

There are many interesting ways to use Promises. The following examples are written with Node.js.

### HTTP Requests

Earlier in this chapter, I mentioned how HTTP requests are asynchronous operations. By using Promises, you would be able to act on the result from the request without having to use callbacks or wait an arbitrarily set amount of time for the response to be returned.

The following code sends a request to an Amazon product URL and retrieves the price of the product using Promises. By using Promises instead of callbacks, we have a nice little web scraping snippet that's easy for anyone to use and to debug if any issues arise.

```javascript

const request = require('request-promise');
const cheerio = require('cheerio');

request(prodLink)
    .then(function (body) {
        const $ = cheerio.load(body);
        let price = $('#priceblock_ourprice').text().trim();
        console.log(price);
    })
    .catch(function (err) {
        console.log(err);
    });

```

### Disk I/O

Reading a file, especially a large one may take some time to complete. If we were to use a synchronous file reading function, the rest of your program wouldn't be able to run because it is stuck waiting for the file to be read. Instead, we can use asynchronous file reading functions which allow for background loading of the file, whilst keeping your program humming along.

```javascript

const fs = require('fs');

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


## Benefits

- Avoid callback hell

![Callback Hell](callbackhell.png)
(Credit: [Brett McLain](http://blog.mclain.ca/))

In Javascript, callback functions are usually used to perform some action on a value returned from another function. However, this may result in deeply nested code as shown above, especially if there are many asynchronous method calls. When using promises, the nesting is kept at a minimum and results in cleaner and more readable code.

- Chain multiple asynchronous methods

Calling asynchronous code can be tedious, especially if a later function relies on data that is produced by an earlier function. By using Promises, these asynchronous methods can be chained and variables will be correctly set instead of being undefined when the variable needs to be used.

- Control execution flow

As mentioned earlier, calling asynchronous methods in Javascript may not necessarily result in a sequential execution flow. However, it is possible to ensure that methods get called in the order that they are written in, as long as they are wrapped in a Promise.

## Other functions

Sometimes multiple promises may have to be used at a time, and Javascript provides excellent support with the `Promise.all` and `Promise.race` functions.

There is an excellent write up on these methods [here](https://davidwalsh.name/promises), which will allow you to use Promises even more efficiently.

In addition, there are other libraries such as [Bluebird](http://bluebirdjs.com/docs/getting-started.html) and [Q](https://github.com/kriskowal/q) which offer even more functionality such as Promise monitoring and synchronous inspection of Promises.

## Further Reading

You may read more about Promises, and how to use them at the following 2 websites:

[JavaScript Promises: an Introduction](https://developers.google.com/web/fundamentals/primers/promises)
[JavaScript Promises for Dummies](https://scotch.io/tutorials/javascript-promises-for-dummies)
