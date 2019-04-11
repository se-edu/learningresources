<frontmatter>
  title: "Javascript: Promises"
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Javascript: Promises

Author: Daniel Berzin Chua, Ong Shu Peng

## Why Promises?

We're often used to operations completing their work before proceeding with other operations in a sequential manner. These sorts of operations are *synchronous* (run one after another), and they're usually easy to understand and debug. However, take an HTTP request for example. It is an operation that takes a while to process, depending on your internet speed and where you are in the world.

If such an operation was to be executed in a synchronous manner, your application would be slow because it has to wait for this request to be resolved by the server and it would not make for a particularly good user experience. Instead, we can make HTTP requests to operate *asynchronously* (many things run at one time) in order to improve the speed and user experience of your program. Asynchronous operations do not wait for their work to be finished before proceeding on with other operations, which allow for them to continue processing in the background while other operations are executed. 

However, asynchronous operations can cause programming and debugging to be difficult, because you would need some sort of way to know when the operation has finished, or in the case of debugging, the point at which the operation is called. It can be especially confusing to trace the code since it may not run in the sequence that you would normally expect.

To illustrate this problem in code, we'll use `setTimeout()`. This is an asynchronous function takes in 3 parameters, a callback, a time (in milliseconds) to wait before the callback is executed and lastly, additional parameters to pass to the *callback*. A callback is a function that is passed as a parameter to another function, and it will be executed after that function finishes. `setTimeout()` is asynchronous as the code below it will execute while the timer is counting down, as you will see in the following code snippet.

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

This is what's infamously known as *callback hell*. It's a natural result of using too many callbacks, as this would result in the code becoming deeply nested. It would be difficult for anyone to read your code and to understand what exactly is going on.

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

## Getting Started 

Here, we will be showing you some sample code to get started with promises. Let's learn how to go from callbacks to promises using a few examples.

### From callbacks to promises
Say we have two time intensive functions, `getData()` and `filterData()` which require some time to complete. You will have to get the data from some server using `getData()`, then process it using `filterData()`, before you can start displaying the results.

How would such functions be implemented using the callback method? The callback method utilizes the fact that we can easily pass functions into javascript as parameters and then use them within the function, effectively "passing" any form of data out, without explicitly returning any value.

We will implement the above use case in the "callback style" (mimicking the long return time of the functions using `setTimeout`):

```javascript
function executeWithDelay(val, callback) {
    // return val after a short wait
    setTimeout(function(){
        callback(val);
    }, 1000);
}

/** The very long calls **/
function getData(callback) {
    executeWithDelay('some random data', callback);
}

function filterData(data, callback) {
    executeWithDelay(data.split(' '), callback);
}

function main() {
    getData(function(data){
        filterData(function(filtered){
            // will print array of splitted text
            console.log(filtered);
        })
    });
}

main();
```

Now we will rewrite all these using promises. We will be using the same function and variable names, this will let you see how exactly promise works, as opposed to the callback style that you might be more familar with.

```javascript
function executeWithDelay(val) {
    // return val after a short wait
    return new Promise(resolve => {
        setTimeout(function(){
            resolve(val);
        }, 1000);
    });
}

/** note these funcs now return a promise **/
function getdata() {
    return executeWithDelay('some random data');
}

function filterdata(data, callback) {
    return executeWithDelay(data.split(' '));
}

function main() {
    return getData()
        .then(data => filterData(data))
        .then(filtered => console.log(filtered));
}

main();
```

Using Promises keep the code clean and easy to read, which makes it easy to learn what the code does, instead of having to trace through the code written with callbacks.

### The imperative style of promises

The "promise style", which uses `.then()` to pass data from one function to the next is comparable to functional programming. The original promise is passed from one `.then()` to the other, and with each `.then()`, a new promise is returned for the next `.then()` to work on.

That is unlike the imperative programming style which is considered to be a more intuitive way of writing code. As such, the `async` and `await` keywords are implemented to facilitate a iterative way of using promises.

The `async` keyword ensures that the function returns a promise, and as for `await`, you can think of it as waiting for a pending promise to be fulfilled before continuing. So back to the same example.

So here is the code in the "promise style", consider the `main` function from the previous example:

```javascript
function main() {
    return getData()
        .then(data => filterData(data))
        .then(fltered => console.log(filtered));
}

main();
```

It can be rewritten in the "async-await" style as follows:

```javascript
async function main() {
    const data = await getData();
    const filtered = await filterData(data);

    console.log(filtered);
    return filtered;
}
```

The `async` keyword ensures that the `main()` function returns a promise. In our case, this will cause `main()` to return a promise with `filtered` as its data. The information can then be used like so `main().then(filtered => alert(filtered));`. 

Another interesting thing to note: `await` will wait for the promise to return before executing anything below. In this case, `console.log` will be executed after the two `await` calls, even when it doesn't depend the results of those calls.

In the "promise" style, we handle errors using the `.catch()` block. However when using the "async await" style, we handle the errors using the more conventional `try ... catch` block. These can be explored further in this [section](https://javascript.info/async-await#error-handling) about error handling.

## Where Promises can be used 
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

You may read more about Promises, and how to use them at the following pages:

- [JavaScript Promises: an Introduction](https://developers.google.com/web/fundamentals/primers/promises)
- [JavaScript Promises for Dummies](https://scotch.io/tutorials/javascript-promises-for-dummies)
- [Javascript Async/Await](https://javascript.info/async-await)

</div>
