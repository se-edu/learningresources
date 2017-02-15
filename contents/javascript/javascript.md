# Writing Testable Javascript

JavaScript is a powerful language. However, its flexibility leads to multiple ways for people to go about doing the same thing. The end result is that multiple collaborators working on a single project can produce different code that does the same thing.

That is why there is a need to follow a standard way of writing JavaScript - it allows for more maintainable cleaner and more beautiful code.

Good JavaScript code should be testable, composable and reusable.

## Writing testable JavaScript

### Avoid coupling with selectors
When writing front end code we would encounter code that manipulates the document object model (DOM). Let's look at one such example.

```js
var DIV_STATUS_MESSAGE = '#statusMessagesToUser';

function populateStatusMessageDiv(message, status) {
    var $statusMessageDivToUser = $(DIV_STATUS_MESSAGE);
    ...
}
```

In this piece of code, we see that the selector is intrinsically tied to the function. This means that the function is tightly tied with the selector and is not easily testable as the test code has to generate the same markup in the test suite for the function to hook up to. In order to prevent such tight coupling, it is advised to leave the selector as a parameter, or sometimes, simply pass in the element itself.

```js
function populateStatusMessage(selector, message, status) {
    var $statusMessageDivToUser = $(selector);
    ...
}
```

### Split business logic and presentation code
When we have to write code that generates markup, it requires business logic. However, mixing the two up is not a good idea. Let's look at the following example:

```js
$.ajax({
    type: 'POST',
    url: '/admin/adminStudentGoogleIdReset?' + params,
    beforeSend: function() {
        $(button).html("<img src='/images/ajax-loader.gif'/>");
    },
    error: function() {
        $(button).html('An Error Occurred, Please Retry');
    },
    success: function(data) {
        ...
    }
});
```

It order to test such a function, we would now have to incorporate both logic and also the mock-up generated. Splitting the logic and markup into two separate functions will both make it easier to test and composable because now you can reuse code that generates the markup in multiple places.

```js
function setLoadingImage(selector) {
    $(selector).html("<img src='/images/ajax-loader.gif'/>");
}

function setErrorText(selector) {
    $(selector).html('An Error Occurred, Please Retry');
}

function displayResults(data) {
    ...
}

$.ajax({
    type: 'POST',
    url: '/admin/adminStudentGoogleIdReset?' + params,
    beforeSend: setLoadingImage,
    error: setErrorText,
    success: displayResults
});
```

Already, we are seeing some of the patterns that lead to the MVC, albeit in a very small scale.

### Avoiding big anonymous functions

Although anonymous functions can lead to cleaner and shorter code, critical business logic should not be written in anonymous functions. The lack of namespace makes them impossible to test. This is common, and tempting when the code starts off in a `document.ready()` or `$.ajax()`.

The testable way of writing such function is to simply give the function a name, which allows it to be tested.

```js
$(document).ready(function() {
    bindStudentPhotoLink('.profile-pic-icon-view-link');
    ...
});

function bindStudentPhotoLink(selector) {
    ...
}
```

Of course, this may be a problem if two Javascript functions have the same name, as they are all in the global scope. This is where the module pattern can be used.

```js
var myApp = (function() {

    var id= 0;

    return {
        next: function() {
            return id++;
        },

        reset: function() {
            id = 0;
        }
    };
})();
```

As demonstrated above, only `myApp` is declared in the global scope, and we can access its methods through the object notation, e.g. `myApp.next()`. This is also especially useful to declare private variables that be used among functions. `id` is not accessible outside of scope in this example.

### Purity is worth pursuing

There are tonnes of literature about functional programming. I will heavily recommend reading the [Mostly adequate guide to Functional Programming](https://github.com/MostlyAdequate/mostly-adequate-guide). It explains functional concepts extremely well.

The benefit of pure functions is simple, there is no need to keep track of state. Given an input, the output is guaranteed to be the same every time. This allows us to write extremely simple unit tests, instead of having to maintain the state while testing.

```js
var counter = 0;
var todos = [];

function getTodo() {
    counter++;
    if (counter < todos.length) {
        return 'NIL';
    }
    return todos[counter];
}
...
getTodo();
```

The above function `getTodo` is not stateless as it depends on counter's value. In order to write the tests, we would need to ensure counter is reset to the same value at the end of each test. A better way would be to do this:

```js
var counter = 0;
var todos = [];

function getTodo(counter, todos) {
    if (counter < todos.length) {
        return 'NIL';
    }
    return todos[counter + 1];
}
...
getTodo(counter, todos);
```

Now, not only that the person who writes the unit test can write in fewer lines of code, you can also use the function for some other state other than the global values.

## Writing reusable javascript

### Optional parameters

When writing certain functions, there would be certain situations where we want to have optional parameters. The easiest way would be to put optional parameters at the end of the parameter calls, like such:

```js
// es5 syntax
function createPopUp(title, content, status, headerColor, bodyColor) {
    var headerColor = headerColor || 'default';
    var bodyColor = bodyColor || 'default';
    ...
}

// es6 syntax default parameters
function createPopUp(title, content, status, headerColor = 'default', bodyColor = 'default') {
    ...
}
```

 This is relatively simple and easy to understand. However, in order to specify body color, the user would have to know and fill in the default header color.

```js
// es5 syntax
function createPopUp(title, content, status, optionals) {
    var headerColor = optionals.headerColor || 'default';
    var bodyColor = optionals.bodyColor || 'default';
    ...
}
// es6 syntax with destructuring and default parameters
function createPopUp(title, content, status, optionals) {
    const { headerColor = 'default', bodyColor = 'default' } = optionals;
    ...
}
```

By using an object, the user would just need to fill in what parameter they want to be changed. In large functions, this would result in better readability as well.

```js
createPopUp('Warning', 'This will delete everything!', dangerStatus, { bodyColor: 'red' });
```

## Resources
[Clean Code Javascript](https://github.com/ryanmcdermott/clean-code-javascript)
Apparently most of what I wrote appears in this huge guide in some form. It's an amazing resource and also explains SOLID clearly near the bottom.

[Airbnb Javascript Style](https://github.com/airbnb/javascript)
It is one thing to follow the style guide, and another to understand why it is that way. Understanding why Airbnb chose certain constructs and syntax reveals ways to write code that is clean, understandable and maintainable.

[JavaScript Design Patterns](https://addyosmani.com/resources/essentialjsdesignpatterns/book/)
The title says it all. From the most common to obscure patterns, this book covers design patterns and explains trade offs. Although it specifically caters to Javascript, it's recommended reading for all prospective software engineers.

## References
[Namespacing in Javascript](https://javascriptweblog.wordpress.com/2010/12/07/namespacing-in-javascript/)
