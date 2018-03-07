# VueJs

Author: Lu Lechuan

A JavaScript framework is an application framework written in JavaScript. It differs from a JavaScript library in its control flow: a library offers functions to be called by its parent code, whereas a framework defines the entire application design.
JavaScript frameworks aim to improve code quality and maintainablity, example of some javascript frameworks include Angular, React and Vue.

## Advantages of VueJs
VueJs is an approachable, versetile and performant framework that helps to create a more maintainable and testable database.

* approachable: VueJs is very easy to learn. Comparing to other framework such as Augular, Vue is simple in term of API and design. Learning enough to build non-trivial applications typically takes less than a day.

* progressive: Vue is designed from the ground up to be incrementally adoptable. The core library is focused on the view layer only, and is easy to pick up and integrate with other libraries or existing projects. This means that if you hava a large application, you can plug Vue into just a part of your application without messing the rest up.

* versetile: Vue is perfectly capable of powering sophisticated Single-Page Applications when used in combination with modern tooling and supporting libraries.

* clean: Vue symtax is simple and this can make the HTML pages very clean.

## Installation

There are many ways to install VueJs.
* Direct `<script>` Include:
  - [development version](https://vuejs.org/js/vue.js)
  - [product version](https://vuejs.org/js/vue.min.js)
* CDN:
  - [cdn](<script src="https://cdn.jsdelivr.net/npm/vue@2.5.13/dist/vue.js"></script>)
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

## The first usage of VueJs:

We first starts our need to display message on our webpage:

```HTML
<body>
  <div id="root">

		<h1>{{ message }}</h1>

	</div>

	<script src="https://unpkg.com/vue@2.5.13/dist/vue.js"></script>

	<script src="the_path_to_the_javacript_file.js"></script>
</body>
```

```js
new Vue ({
	el: '#root',

  data: {
    message: Hello World
  }
});
```

Explanation:
In the HTML file, we first import Vue cdn, and also the JavaScript file which include our Vue codes.
In the JavaScript file, we need to create a Vue instance, and bind this instance to one of the component in our html file (the `root` element). In this case, only this component is working in Vue while the rest are unaffected. This is how we progressively plug in Vue into our projects without a complete one at a go.
In the Vue instance, we specify our data. In this case we define a message to be "Hello World", then we pass this message to the html file using double curly brackets.
Remember, we can only use the vue data inside the component with id "root" as we bind it to the Vue element.
Open the brower and we will see "Hello World" being displayed.

More usages of VueJs please read the other documents in the other files of the current folder.
