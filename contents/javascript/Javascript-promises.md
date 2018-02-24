# Javascript: Promises

Author: Daniel Berzin Chua

In Javascript, there are occasions where a method call may not resolve instantly and as such, the code may not run in the sequence that you would expect due this asynchronous nature. It can be especially confusing for beginners learning Javascript.

A pertinent example would be HTTP requests. When making a HTTP request, the information from the server is not instantly returned as there is a round trip time that involves the time taken for the request to be sent, and for the response from the server to be sent back to the client. In this time, Javascript would not necessarily wait for the response to be received, but instead execute other functions first, even if those functions depend on the result of that HTTP response.

In the code snippet below, `console.log(price)` would return undefined as the asynchronous `retrievePrice()` function has not finished executing.

```
var price;
price = retrievePrice("https://www.amazon.com/gp/product/B009S331VU/");
console.log(price);

```

As such, it can be difficult to predict how your Javascript code executes as asynchronous methods such as the above would not necessarily execute in the sequence that is desired. 

This is where Promises would come in handy.


## Introduction
Promises in Javscript behave the same way as Promises do in real life. It is a proxy for a value that is not known at the time of Promise creation. It has 3 states: pending, fulfilled and rejected. Promises provide the power to control the execution flow of your program by allowing the user to wait for a promise to be resolved before carrying on with the rest of the program execution.

## Benefits

- Avoid callback hell

![Callback Hell](http://blog.mclain.ca/assets/images/callbackhell.png)

In Javascript, callback functions are usually used to perform some action on a value returned from another function. However, this may result in deeply nested code as shown above, especially if there are many asynchronous method calls. When using promises, the nesting is kept at a minimum and results in cleaner and readable code as shown below. 

- Chain multiple asynchronous methods

Calling asynchronous code can be tedious, especially if a later function relies on data that is produced by an earlier function. By using Promises, these asynchronous methods can be chained and variables will be correctly set instead of being undefined when the variable needs to be used.

- Control execution flow

As mentioned earlier, calling asynchronous methods in Javascript may not necessarily result in a sequential execution flow. However, it is possible to ensure that methods get called in the order that they are written in, as long as they are wrapped in a Promise.


## How

Promises are created in Javascript by using the `new` constructor as follows:

```

var myFirstPromise = new Promise(function(resolve, reject) {
    // insert stuff to do here
    // The promise is currently 'pending'
})

```

The Promise constructor takes function that has two parameters: resolve and reject. These two parameters are functions which will change the state of the promise. They can be called with or without a parameter as follows:

```

resolve(someValue);
reject(someValue);
resolve();
reject();

```

## Handling

As promises have 2 states, there are 2 functions available to deal with these 2 scenarios. 

### .then

`.then` is used to deal with a Promise after it has been resolved. It takes in a function with 1 parameter, which is the result of the Promise that has just been resolved.

### .catch

`.catch` is used to deal with a Promise after it has been rejected. Like `.then`, it also takes in a function with 1 parameter.

Whenever you want to act on a result from a Promise, it is good practice to use the above 2 functions to ensure certainty and code readability.

An example would be as follows:

```

var isSuccessful = false;

var myFirstPromise = new Promise(function(resolve, reject) {
    if (isSuccessful)
        resolve('Success');
    else
        reject('Failure');
}).then(function(result) {
    console.log(result);    // prints 'Success'
}).catch(function(result) {
    console.log(result);    // prints 'Failure'
});

```

## Other functions

Sometimes multiple promises may have to be used at a time, and Javascript provides excellent support with the `Promise.all` and `Promise.race` functions.

There is an excellent write up on these methods [here](https://davidwalsh.name/promises), which will allow you to use Promises even more efficiently.