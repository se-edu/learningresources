# VueJs

Author: [Lu Lechuan](https://github.com/LuLechuan)

VueJs (also known as Vue) is an open-source progressive [JavaScript framework](https://en.wikipedia.org/wiki/JavaScript_framework) for building user interfaces. It is designed to improve code quality and maintainablity.

## Creating a Simple Project in VueJs

Printing the VueJs version of "Hello World" (a minimal example to show how easy it is to integrate VueJs into your web project):

```HTML
<!--This is the main HTML file-->
<body>
  <div id="root">
    <h2>{{ message }}</h2>
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

Step by step guide:

1. Import VueJs CDN and the JavaScript file in the main HTML file.
```HTML
  <script src="https://unpkg.com/vue@2.5.13/dist/vue.js"></script>
  <script src="the_path_to_the_javacript_file.js"></script>
```

2. Create a Vue instance in the JavaScript file; bind the instance to one of the component in our html file (the `root` element).
  - In this case, only the `root` component can be accessed in VueJs while the rest are unaffected. This is how we progressively plug in VueJs into our projects.
```js
  new Vue ({
    el: '#root',
  });
```
```HTML
  <div id="root"></div>
```

3. Specify our data (message: "Hello World") in the Vue instance.
```js
  data: {
    message: "Hello World"
  }
```

5. Pass the message to the HTML file using double curly brackets.
```HTML
  <div id="root">
    <h2>{{ message }}</h2>
  </div>
```

6. Open the brower and we will see "Hello World" being displayed

## Advantages of VueJs

Here are some important advantages of VueJs:

1. <b>Approachable:</b>
VueJs is very easy to learn. Compared to other framework such as Angular and ReactJs, VueJs is simple in term of API and design. Learning enough to build non-trivial applications typically takes less than a day. An example is provided below:
> How is iteration like in ReactJs:
>
```js
// ReactJs (jsx)
var Iteration = React.createClass({
   getInitialState() {
     return {
       array: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
     }
   },
   render() {
     this.state.array.map(function(date) {
       return (
         <span>{date}</span>
       )
     });
   }
});
ReactDOM.render(<Iteration />, document.getElementById('array'));
```
```html
<!-- ReactJs (html) -->
<div id="array"></div>
```
>  How is iteration like in VueJs:
>
```js
// VueJs (js)
var Iteration = new Vue({
  el: '#array',
  data: {
    array: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
  }
});
```
```html
<!-- VueJs (html) -->
<div id="array">
  <span v-for="date in array">{{date}}</span>
</div>
```

2. <b>Progressive:</b>
VueJs is designed from the ground up to be incrementally adoptable. The core library is focused on the view layer only, and is easy to pick up and integrate with other libraries or existing projects. This means that if you have a large application, you can plug VueJs into just a part of your application without disturbing the other components. A quote from Evan You - the founder of VueJs is as follow:
> Vue.js is a more flexible, less opinionated solution (than Angular). That allows you to structure your app the way you want it to be, instead of being forced to do everything the Angular way (Angular requires a certain way to structure an application, making it hard to introduce Angular into an already built project). It’s only an interface layer so you can use it as a light feature in pages instead of a full blown SPA (single-page application).
>
> -- <cite>Evan You</cite> [[source]](https://www.valuecoders.com/blog/technology-and-apps/vue-js-comparison-angular-react/)

3. <b>Versatile:</b>
VueJs is perfectly capable of powering sophisticated single-page applications when used in combination with modern tooling and supporting libraries.

4. <b>Clean:</b>
VueJs syntax is simple and this can make the HTML pages very clean. This would allow user interfaces built by VueJs to be more maintainable and testable.

## Drawbacks of VueJS

1. <b>Relatively small size community:</b>
VueJs is a relatively new JavaScript framework as compared to Angular and React. The size of the community for VueJs is therefore relatively small. Although small size community means you can differentiate yourself from other JavaScript developers, it also means there are fewer resources such as tutorials and problem-shooting guides.

2. <b>Language barriers:</b>
A majority of users of VueJs are the Chinese as VueJs is developed by a Chinese American. He is supportive of the Chinese community and hence a lot of the existing plugins are written in Chinese. There might be some language barriers for an English speaking developer seeking for VueJs resources.

## Other Advantages and Drawbacks of VueJs
Here are some of many articles that describe advantages and drawbacks of VueJs:
- [What is VueJs and What are its Advantages](https://hackernoon.com/what-is-vue-js-and-what-are-its-advantages-4071b7c7993d)
- [Pros and Cons of the Vue.js framework - Vue.js Feed](https://vuejsfeed.com/blog/pros-and-cons-of-the-vue-js-framework)

Detailed Comparison of VueJs with other JavaScript frameworks can be found from:
- [Vue Guild: Comparison with Other Frameworks](https://vuejs.org/v2/guide/comparison.html)
- [Angular vs React vs Vue](https://medium.com/unicorn-supplies/angular-vs-react-vs-vue-a-2017-comparison-c5c52d620176)

## Installation

There are [multiple ways to install VueJs](https://vuejs.org/v2/guide/installation.html) and they are all easy. For example, one of the easiest ways is to run the npm command `$ npm install vue`.

Apart from installing VueJs, you can install [VueJs development tools](https://github.com/vuejs/vue-devtools#vue-devtools) in your browser. This will allow you to inspect and debug the Vue components in your projects.

## Links to VueJs tutorials and practices

- [VueSchool](https://vuejs.org/)
- [Laracast](https://laracasts.com/series/learn-vue-2-step-by-step)
- [Vuetify](https://vuetifyjs.com/zh-Hans/)

## References

- [Inversion of Control](http://martinfowler.com/bliki/InversionOfControl.html)
- [Differences Between Library and Framework](http://www.c-sharpcorner.com/UploadFile/a85b23/framework-vs-library/)
- [VueJS Official Website](https://vuejs.org/)
