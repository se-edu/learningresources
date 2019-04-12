<frontmatter>
  title: VueJs
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# VueJs

**Authors**: [Lu Lechuan](https://github.com/LuLechuan), [Chelsey Ong](https://github.com/chelseyong)<br>
Reviewers: [Ong Shu Peng](https://github.com/ongspxm), [Gilbert Emerson](https://github.com/emer7)

VueJs (also known as Vue) is an open-source [JavaScript framework](https://en.wikipedia.org/wiki/JavaScript_framework) for building user interfaces. It is designed to improve code quality and maintainability.

<box type="info">
Reader should have basic knowledge of HTML and JavaScript before proceeding with this article.
</box>

## Creating a Simple Project in VueJs

#### Installation

There are [multiple ways to install VueJs](https://vuejs.org/v2/guide/installation.html) and they are all easy. For example, one of the easiest ways is to run the npm command `$ npm install vue`.

Apart from installing VueJs, you can install [VueJs development tools](https://github.com/vuejs/vue-devtools#vue-devtools) in your browser. This will allow you to inspect and debug the components that use VueJs in your projects.

#### HelloWorld in VueJs

This is a simple example to show how easy it is to integrate VueJs into your web project:<br/><br/>
The main HTML file:
```HTML
<body>
  <div id="root">
    <h2>{\{ message }\}</h2>
  </div>
  <script src="https://unpkg.com/vue@2.5.13/dist/vue.js"></script>
  <script src="the_path_to_the_javacript_file.js"></script>
</body>
```
This is inside the javacript file:
```js
new Vue ({
  el: '#root',

  data: {
    message: "Hello World"
  }
});
```
<box type="warning">
Note that `{\{` and `}\}` should not have the slash in your actual code.
</box>

Step-by-step explanation of the code:

<b>Step 1:</b> Import VueJs CDN and the JavaScript file in the main HTML file.
```HTML
  <script src="https://unpkg.com/vue@2.5.13/dist/vue.js"></script>
  <script src="the_path_to_the_javacript_file.js"></script>
```

<b>Step 2:</b> Create an instance of Vue (Vue is an object) in the JavaScript file; bind the instance to one of the component in our html file (e.g. create a component with id `root` and bind it with the instance of Vue).<br/>
In this case, only the `root` component can be accessed in VueJs while the rest are unaffected. This is how we progressively plug in VueJs into our projects.

```js
  new Vue ({
    el: '#root',
  });
```
```HTML
  <div id="root"></div>
```

<b>Step 3:</b> Specify our data (message: "Hello World") in the instance of Vue Class.
```js
  data: {
    message: "Hello World"
  }
```

<b>Step 4:</b> Pass the message to the HTML file using double curly brackets.
```HTML
  <div id="root">
    <h2>{\{message}\{</h2>
  </div>
```

<b>Step 5:</b> Open the brower and we will see "Hello World" being displayed:
> <h2>Hello World</h2>
<br>

-----
### Vue's Features
Here are some features to get you familiar with Vue:
1. **Mutating of data in the DOM**<br>
    In Vue, the state of the data can be directly modified.

    Let's say, there is a variable called `message` in your app. To modify `message`, you can do it as such:
    ```js
    this.message = 'Hello Space';
    ```
    When `message` is changed, the view will be re-rendered to show the new message. So you can say, DOM is "reacting" to `message`.

    In situations when you want to tie data to the view, do ensure that this data already has an initial value, so that any subsequent updates to this data will enable view to update itself
    ```js
    data: {
        this.message = '';
    }
    ```


<br>

2. **2-way binding**<br>
    `v-model` is a [Vue directive](https://vuejs.org/v2/api/#v-model) used to bind the DOM input field to its data variable.

    This effectively allows the DOM variables and data to be "in sync", regardless of which one is being updated first.
    In other words, if you change the input value, the bound data will change, vice versa.

    This would also reduce any extra step required to manually update the data.
    ```html
    <input type="checkbox", v-model=isChecked">
        <label for="checked">Select</label>
    ```

    2-way binding is generally used for input form bindings such as checkboxes or drop-downs, where new data is entered by users and then updated in the view.

<br>

3. **1-way data flow**<br>
    When you have components that are nested within each other, data can only be passed from the outer component to the inner component, via `props`, where `props` are just custom data shared between the components.

    The outer and inner components are also called parent and child, respectively.

    On the other hand, if the inner component wants to pass data to the outer component, `$emit` events have to be used instead.

    In the example below, `to-do list` contains `item`, which means `to-do list` is the outer component (parent) and `item` is the inner component (child).

    The data in `item` is being passed to `todo-list` for rendering. (`item` -> `todo-list`)
    ```js
    Vue.component('todo-list', {
        props: ['item'],
        data: ['totalCount'],
        template:
          <div class='todo-list'>
            <p>Total:{\{this.totalCount}\{</p>
            <p>{\{item.name}\{: {\{item.count}\{</p>
    })

    <todo-list
      v-for='item in items'
      v-bind:key='item.id'
      v-bind:item='item'
    ></todo-list>
    ```
    <box type="tip">
        Difference between props and data: props is passed from the parent to the child while data is kept private within a component
    </box>

    However, what if the user decides to update the `item.count`? The data for `item.count` has to be passed from `item` to `todo-list` so that `totalCount` can be updated inside `todo-list` .

    Under situations like this where the inner component has to pass data back to the outer component, the inner component has to [emit custom events](https://vuejs.org/v2/guide/components.html#Emitting-a-Value-With-an-Event)
    and the outer component will update after listening to these events.

    ```js
    Vue.component('item', {
      data: ['count', 'name'],
      template: {
        <button v-on:click="$emit('increased-count', count+1)">Increment item count</button>
      }
    }

    /* Inside todo-list component */
    template: {
        v-on:increased-count="updateCount"
    }
    ```
    Whenever the button is pressed, a custom event called `increased-count` will contain the new value of `count` and be emitted by the `item` (child).
    When `todo-list` (parent) listens to the event, it will call `updateCount`.

    Using 1-way data flow ensures that the data can only be changed by the component itself and allows bugs to be easily traced in the code.

<br>


4. **Computed properties**<br>
    This is useful when you want to reduce the amount of logic written in templates.
    Using the example from above, we can convert `totalCount` into a computed property.

    ```js
    computed: totalCount() {
        let result = 0
        this.items.forEach((item) => result += item.count);
        return result;
    }
    ```

    Unlike the use of methods, this updating of `totalCount` will only be triggered when the number of `items` in the list or any `item`'s `count` changed.

    Since computed properties are cached and will not be processed every time the page refreshes, this can greatly improve the efficiency of your application.

    <box type="warning">
        Note: computed properties must return the new data i.e. reactive properties.
        It cannot perform other operations in response to the change in data.
    </box>

<br>

5. **Watched properties**<br>
    This is another useful feature that is quite similar to `Computed properties`.

    While computed properties are more appropriate to use when you want to update a new state or data,
    watched properties are generally used to run other functions when this particular data has changed.

    Watched property | Computed property
    :-------------- | :----------------
    executed every time page refreshes | uses cached data and executes only when changed
    allow 1 variable to observed only | allow multiple variables to be observed

    If I switch computed property to watched property from the previous example, it will look like this:

    ```js
    watch: {
        totalCount: function() {
            let result = 0
            this.items.forEach((item) => result += item.count);
            this.totalCount = result;
        }
    }
    ```
    In this case, `this.totalCount` has to be updated inside the function, instead of returning the new result of `totalCount`.
    As such, computed properties is more intuitive to use here, where a state is updated automatically with new data.

    Whereas, watched properties are commonly used to perform asynchronous operations (more details in JavaScript Promises) when a particular data has changed.
    Such situations could happen when a new `item` is added and I want to send an update to my friend to inform that a new `item` is added.
    A watched property on `items` can be added so that a network request can be sent whenever `items` has changed.

<br>

---
## Advantages and Disadvantages of VueJs

#### Advantages of VueJs

1. **Approachable:**<br/>
VueJs is very easy to learn. Compared to other framework such as Angular and ReactJs, VueJs is simple in term of API and design. Learning enough to build non-trivial applications typically takes less than a day. An example is provided below:<br/><br/>
How is iteration like in ReactJs:<br/><br/>
The JavaScript file in ReactJs
    ```js
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
    The HTML file in ReactJs
    ```html
    <div id="array"></div>
    ```
    How is iteration like in VueJs:<br/><br/>
    The JavaScript file in VueJs
    ```js
    var Iteration = new Vue({
      el: '#array',
      data: {
        array: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
      }
    });
    ```
    The HTML file in VueJs
    ```html
    <div id="array">
      <span v-for="date in array">{date}</span>
    </div>
    ```

2. **Progressive:**<br/>
VueJs is designed from the ground up to be incrementally adoptable. The core library is focused on the view layer only, and is easy to pick up and integrate with other libraries or existing projects. This means that if you have a large application, you can plug VueJs into just a part of your application without disturbing the other components. A quote from Evan You - the founder of VueJs is as follow:
    > Vue.js is a more flexible, less opinionated solution (than Angular). That allows you to structure your app the way you want it to be, instead of being forced to do everything the Angular way (Angular requires a certain way to structure an application, making it hard to introduce Angular into an already built project). It’s only an interface layer so you can use it as a light feature in pages instead of a full blown SPA (single-page application).
    >
    > -- [[source]](https://www.valuecoders.com/blog/technology-and-apps/vue-js-comparison-angular-react/)

3. **Versatile:**<br/>
VueJs is perfectly capable of powering sophisticated single-page applications when used in combination with modern tooling and supporting libraries.

4. **Clean:**<br/>
VueJs syntax is simple and this can make the HTML pages very clean. This would allow user interfaces built by VueJs to be more maintainable and testable.

#### Disadvantages of VueJs

1. **Relatively small size community:**<br/>
VueJs is a relatively new JavaScript framework as compared to Angular and React. The size of the community for VueJs is therefore relatively small. Although small size community means you can differentiate yourself from other JavaScript developers, it also means there are fewer resources such as tutorials and problem-shooting guides.

2. **Language barriers:**<br/>
A majority of users of VueJs are the Chinese as VueJs is developed by a Chinese American. He is supportive of the Chinese community and hence a lot of the existing plugins are written in Chinese. There might be some language barriers for an English speaking developer seeking for VueJs resources.

---
### Detailed Comparison of VueJs with other JavaScript frameworks can be found from:
- [Vue Guild: Comparison with Other Frameworks](https://vuejs.org/v2/guide/comparison.html)
- [Angular vs React vs Vue](https://medium.com/unicorn-supplies/angular-vs-react-vs-vue-a-2017-comparison-c5c52d620176)

### Links to VueJs tutorials and practices

- [VueSchool](https://vuejs.org/)
- [Laracast](https://laracasts.com/series/learn-vue-2-step-by-step)
- [Vuetify](https://vuetifyjs.com/zh-Hans/)

### References

- [VueJS Official Website](https://vuejs.org)

</div>
