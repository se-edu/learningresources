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

In this piece of code we see that the selector is intrinsically tied to the function. This means that the function is tightly tied with the selector and is not easily testable as the test code has to generate the same markup in the test suite for the function to hook up to. In order to prevent such tight coupling, it is advised to leave the selector as a parameter, or sometimes, simply pass in the element itself.

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

It order to test such a function, we would now have to incooperate both logic and also the mock-up generated. Splitting the logic and markup in two separate functions will both make it easier to test and composable because now you can reuse code that generates the markup in multiple places. 

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

Already, we are seeing some of pattern, leading to the MVC, albeit in a very small scale.

### Avoiding big anonymous functions

Although anonymous functions can lead to cleaner and shorter code, critical business logic should not be written in anonymous functions. The lack of namespace make them impossible to test. This is common, and tempting when the code starts off in a `document.ready()` or `$.ajax()`.

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

## References
[Namespacing in Javascript](https://javascriptweblog.wordpress.com/2010/12/07/namespacing-in-javascript/)
