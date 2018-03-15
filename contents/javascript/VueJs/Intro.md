# VueJs

Author: Lu Lechuan

Vue.js is an open-source progressive [JavaScript framework](https://en.wikipedia.org/wiki/JavaScript_framework) for building user interfaces. It is designed to improve code quality and maintainablity.

## Advantages of VueJs
VueJs is an approachable, versatile and performant framework that helps to create a more maintainable and testable database.

* Approachable: VueJs is very easy to learn. Comparing to other framework such as Augular, Vue is simple in term of API and design. Learning enough to build non-trivial applications typically takes less than a day.

* Progressive: Vue is designed from the ground up to be incrementally adoptable. The core library is focused on the view layer only, and is easy to pick up and integrate with other libraries or existing projects. This means that if you have a large application, you can plug Vue into just a part of your application without disturbing the other components. A quote from Evan You - the owner of Vue is as follow:
Vue.js is a more flexible, less opinionated solution ( than Angular ). That allows you to structure your app the way you want it to be, instead of being forced to do everything the Angular way. It’s only an interface layer so you can use it as a light feature in pages instead of a full blown SPA.

* Versetile: Vue is perfectly capable of powering sophisticated single-page applications when used in combination with modern tooling and supporting libraries.

* Clean: Vue symtax is simple and this can make the HTML pages very clean.

* Other Advantages of VueJs: [What is Vue.js and What are its Advantages](https://hackernoon.com/what-is-vue-js-and-what-are-its-advantages-4071b7c7993d)

* Detailed comparison of Vue with other JavaScript frameworks can be found from [Vue Guild: Comparison with Other Frameworks](https://vuejs.org/v2/guide/comparison.html).

## Installation

There are many ways to install VueJs, some of the following ways are mentioned below:
* Import Vue in HTML `<script>` tag:
  - [development version](https://vuejs.org/js/vue.js)
  - [product version](https://vuejs.org/js/vue.min.js)
* CDN:
  - include the following into the html file:
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.13/dist/vue.js"></script>
* NPM:
  - $ npm install vue
* CLI:
```cli
    # install vue-cli
    $ npm install --global vue-cli
    # create a new project using the "webpack" template
    $ vue init webpack my-project
    # install dependencies and go!
    $ cd my-project
    $ npm install
    $ npm run dev
```

Apart from installing VueJs, you can install VueJs development tools in your browser. This will allow you to inspect and debug the Vue components in your projects. The link is as follow:
- [Vue DevTools](https://github.com/vuejs/vue-devtools#vue-devtools)

## Creating a Simple Project in VueJs:

The VueJs version of "Hello World":

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
    message: Hello World
  }
});
```

Explanation:
* Import Vue cdn and the JavaScript file in the main HTML file.
* Create a Vue instance in the JavaScript file; bind the instance to one of the component in our html file (the `root` element). (In this case, only this component is working in Vue while the rest are unaffected. This is how we progressively plug in Vue into our projects without a complete one at a go.)
* Specify our data(message: "Hello World") in the Vue instance.
* Pass the message to the HTML file using double curly brackets.
Remark: we can only use the Vue data inside the component with id root as we bind it to the Vue element.
Open the brower and we will see "Hello World" being displayed.

## Links to VueJs tutorials
[VueSchool](https://vuejs.org/)
[Laracast](https://laracasts.com/series/learn-vue-2-step-by-step)
[Vuetify](https://vuetifyjs.com/zh-Hans/)

## References

- [Inversion of Control](http://martinfowler.com/bliki/InversionOfControl.html)
- [Differences Between Library and Framework](http://www.c-sharpcorner.com/UploadFile/a85b23/framework-vs-library/)
- [VueJS Official Website](https://vuejs.org/)
