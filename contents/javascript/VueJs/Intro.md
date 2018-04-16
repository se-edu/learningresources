# VueJs

Author: [Lu Lechuan](https://github.com/LuLechuan)

VueJs (also known as Vue) is an open-source progressive [JavaScript framework](https://en.wikipedia.org/wiki/JavaScript_framework) for building user interfaces. It is designed to improve code quality and maintainablity. Simply put, it is used as a tool to write JavaScript fast and easy.

## Advantages of VueJs

#### Approachable

VueJs is very easy to learn. Compared to other framework such as Angular, VueJs is simple in term of API and design. Learning enough to build non-trivial applications typically takes less than a day.

#### Progressive

VueJs is designed from the ground up to be incrementally adoptable. The core library is focused on the view layer only, and is easy to pick up and integrate with other libraries or existing projects. This means that if you have a large application, you can plug VueJs into just a part of your application without disturbing the other components. A quote from Evan You - the owner of VueJs is as follow:

> Vue.js is a more flexible, less opinionated solution ( than Angular ). That allows you to structure your app the way you want it to be, instead of being forced to do everything the Angular way ( Angular requires a certain way to structure an application, making it hard to introduce Angular into an already built project ). It’s only an interface layer so you can use it as a light feature in pages instead of a full blown SPA ( single-page application ).

#### Versatile

VueJs is perfectly capable of powering sophisticated single-page applications when used in combination with modern tooling and supporting libraries.

#### Clean
VueJs syntax is simple and this can make the HTML pages very clean. This would allow user interfaces built by VueJs to be more maintainable and testable.

#### Other Advantages of VueJs
There are many articles talking about VueJs's pros and cons; some links can be found here:
- [What is VueJs and What are its Advantages](https://hackernoon.com/what-is-vue-js-and-what-are-its-advantages-4071b7c7993d)
- [Pros and Cons of the Vue.js framework - Vue.js Feed](https://vuejsfeed.com/blog/pros-and-cons-of-the-vue-js-framework)

Detailed Comparison of VueJs with other JavaScript frameworks can be found from:
- [Vue Guild: Comparison with Other Frameworks](https://vuejs.org/v2/guide/comparison.html)
- [Angular vs React vs Vue](https://medium.com/unicorn-supplies/angular-vs-react-vs-vue-a-2017-comparison-c5c52d620176)

## Drawbacks of VueJS

#### Relatively small size community

VueJs is a relatively new JavaScript framework as compared to Angular and React. The size of the community for VueJs is therefore relatively small. Although small size community means you can differentiate yourself from other JavaScript developers, it also means there are fewer resources such as tutorials and problem-shooting guides.

#### Language barriers

A majority of users of VueJs are the Chinese as VueJs is developed by a Chinese American. He is supportive of the Chinese community and hence a lot of the existing plugins are written in Chinese. There might be some language barriers for an English speaking developer seeking for VueJs resources.

## Installation

There are many ways to install VueJs. Some of the following ways are mentioned below:
* Import VueJs in HTML `<script>` tag:
  - [development version](https://vuejs.org/js/vue.js)
  - [product version](https://vuejs.org/js/vue.min.js)
* CDN:
  - include the following into the html file:
    `<script src="https://cdn.jsdelivr.net/npm/vue@2.5.13/dist/vue.js"</script>`
* NPM:
```npm
    $ npm install vue
```
* CLI:
```cli
    $ npm install --global vue-cli  # install vue-cli
    $ vue init webpack my-project  # create a new project using the "webpack" template
    $ cd my-project  # install dependencies and go!
    $ npm install
    $ npm run dev
```

Apart from installing VueJs, you can install [VueJs development tools](https://github.com/vuejs/vue-devtools#vue-devtools) in your browser. This will allow you to inspect and debug the Vue components in your projects.

## Creating a Simple Project in VueJs

Steps to print the VueJs version of "Hello World" (a minimal example to show how easy it is to integrate VueJs into your web project):

* Import VueJs CDN and the JavaScript file in the main HTML file.
```HTML
  <script src="https://unpkg.com/vue@2.5.13/dist/vue.js"></script>
  <script src="the_path_to_the_javacript_file.js"></script>
```

* Create a Vue instance in the JavaScript file; bind the instance to one of the component in our html file (the `root` element).
  - In this case, only the `root` component can be accessed in VueJs while the rest are unaffected. This is how we progressively plug in VueJs into our projects.
```js
  new Vue ({
  	el: '#root',
  });
```
```HTML
  <div id="root"></div>
```

* Specify our data (message: "Hello World") in the Vue instance.
```js
  data: {
    message: "Hello World"
  }
```

* Pass the message to the HTML file using double curly brackets.
```HTML
  <div id="root">
    <h1>{{ message }}</h1>
  </div>
```

* Open the brower and we will see "Hello World" being displayed.

Putting all parts together:

```HTML
<!--This is the main HTML file-->
<body>
  <div id="root">
		<h1>{{ message }}</h1>
	</div>
	<script src="https://unpkg.com/vue@2.5.13/dist/vue.js"></script>
	<script src="the_path_to_the_javacript_file.js"></script>
</body>
```

```js
// This is inside the javacript file
new Vue ({
	el: '#root',

  data: {
    message: "Hello World"
  }
});
```

### Common VueJs syntax

#### [Text](https://vuejs.org/v2/guide/syntax.html#Text)

The most basic data binding is text interpolation using double curly brackets:
```HTML
<!--This is the main HTML file-->
<h1>{{ message }}</h1>
```

```js
// This is inside the javacript file
new Vue ({
	el: '#root',

  data: {
    message: "Hello World"
  }
});
```
The double curly brackets with contents inside will be replaced with the value of the `message` property on the corresponding data object. It will also be updated whenever the `data` object’s `message` property changes.

#### [Attributes](https://vuejs.org/v2/guide/syntax.html#Attributes)

Double curly brackets does not work in a HTML attributes. Instead, use a `v-bind` directive.
```HTML
<div v-bind:id="dynamicId"></div>
```
In the case of boolean attributes, where their mere existence implies true, v-bind works a little differently. In this example:
```HTML
<button v-bind:disabled="isButtonDisabled">Button</button>
```
If `isButtonDisabled` has the value of `null`, `undefined`, or `false`, the disabled attribute will not even be included in the rendered `<button>` element.

#### [Directives](https://vuejs.org/v2/guide/syntax.html#Directives)

Directives are special attributes with the `v-` prefix. Directive attribute values are expected to be a single JavaScript expression (with the exception for `v-for`). A directive’s job is to reactively apply side effects to the DOM when the value of its expression changes.
Example of how to use the directives are as follow:

  - `v-example="value"` - this will pass a value into the directive, and the directive will response according to the value (`v-` is the prefix for a directive, `example` is a directive name. Directive attributes includes `v-if`, `v-for`, `v-on` etc. More examples of VueJs directives and their usages are [here](https://012.vuejs.org/api/directives.html)).
  ```HTML
  <div v-if="condition">This div show up if "condition" is true</div>
  ```

  - `v-example="'string'"` - this will let you use a string as an expression.
  ```HTML
  <p v-html="'<b>This example string is in some text and it is bolded</b>'"></p>
  ```

  - `v-example:arg="value"` - this allows us to pass in an argument to the directive. Using the `v-bind` example above, we are binding the `div` to an `id`.
  ```HTML
  <div v-bind:id="dynamicId"></div>
  ```

  - `v-example:arg.modifier="value"` - this allows us to use a modifier. The example below allows us to call `preventDefault()` on the click event to prevent the submission of a form.
  ```HTML
  <button v-on:submit.prevent="onSubmit"></button>
  ```

To know more about syntaxes for VueJs, you can visit the [official website of Vue](https://vuejs.org/).
Of course, knowing the syntaxes is clearly not enough to understand the whole picture of VueJs and use it to build a non-trivial applications. To really master and reap the benefit of VueJs, you will need some [hands-on experiences](#links-to-vuejs-tutorials-and-practices).

## Links to VueJs tutorials and practices

- [VueSchool](https://vuejs.org/)
- [Laracast](https://laracasts.com/series/learn-vue-2-step-by-step)
- [Vuetify](https://vuetifyjs.com/zh-Hans/)

## References

- [Inversion of Control](http://martinfowler.com/bliki/InversionOfControl.html)
- [Differences Between Library and Framework](http://www.c-sharpcorner.com/UploadFile/a85b23/framework-vs-library/)
- [VueJS Official Website](https://vuejs.org/)
