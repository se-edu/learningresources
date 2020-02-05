<frontmatter>
  title: Introductio to Vue
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Introducion to Vue

**Authors: [Chelsey Ong](https://github.com/chelseyong), [Lu Lechuan](https://github.com/LuLechuan)** <br>
Reviewers: [Gilbert Emerson](https://github.com/emer7), [Ong Shu Peng](https://github.com/ongspxm)

<box type="info">
This chapter assumes that the reader has a basic knowledge of HTML and JavaScript.
</box>

## What is Vue?

>*Vue* (also known as Vue) is an open-source [JavaScript framework](https://en.wikipedia.org/wiki/JavaScript_framework) for building user interfaces. It is designed to improve code quality and maintainability.

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
This is inside the JavaScript file:
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

<b>Step 1:</b> Import Vue <tooltip content="Content Delivery Network" placement="top">CDN</tooltip> and the JavaScript file in the main HTML file.
```HTML
  <script src="https://unpkg.com/vue@2.5.13/dist/vue.js"></script>
  <script src="the_path_to_the_javacript_file.js"></script>
```

<b>Step 2:</b> Create an instance of Vue (Vue is an object) in the JavaScript file; bind the instance to one of the component in our html file (e.g. create a component with id `root` and bind it with the instance of Vue).<br/>
In this case, only the `root` component can be accessed in Vue while the rest are unaffected. This is how we progressively plug in Vue into our projects.

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

## Vue Features

1. **Mutating of data in the DOM**<br>
    In Vue, the state of the data can be directly modified.

    Let's say, there is a variable called `message` in your app. To modify `message`, you can do the following:
    ```js
    this.message = 'Hello Space';
    ```
    When `message` is changed, the view will be re-rendered to show the new message. So you can say, DOM is "reacting" to `message`.

<br>

2. **2-way binding**<br>
    `v-model` is a [Vue directive](https://vuejs.org/v2/api/#v-model) used to bind the DOM input field to its data variable.

    This allows the DOM variables and data to be "in sync", regardless of which one is being updated first.
    In other words, if you change the input value, the bound data will change, and vice versa.

    ```html
    <input type="checkbox", v-model=isChecked">
        <label for="checked">Select</label>
    ```

    When the checkbox is selected, `isChecked` is set to `true`. If the program sets `isChecked` to `false`, then checkbox will be unselected.
    This reduces any extra step required to manually update the data.

    2-way binding is useful for updating input form bindings such as checkboxes or drop-downs, where new data is entered by users and then updated in the view.

<br>

3. **Passing data from outer to inner components**<br>
    When you have components that are nested within each other, data is passed from the outer component to the inner component via `props`, where `props` are just custom data shared between the components.

    This follows the [1-way data flow](https://vuejs.org/v2/guide/components-props.html#One-Way-Data-Flow) encouraged by Vue, which
    ensures that data can only be changed by the component itself and also allows bugs to be easily traced in the code.

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
    `to-do list` contains `item`, i.e. `to-do list` is the outer component and `item` is the inner component.

    <box type="tip">

    Note that `props` is passed from the outer component to the inner component while `data` is kept private within a component.
    </box>

<br>

4. **Emitting events**<br>
    However, what if the user decides to update the `item.count`? The data for `item.count` has to be passed from `item` to `todo-list` so that `totalCount` can be updated inside `todo-list` .

    How do we do that if we have to follow the 1-way data flow rule?

    In situations where the inner component has to pass data back to the outer component, the inner component has to [emit custom events](https://vuejs.org/v2/guide/components.html#Emitting-a-Value-With-an-Event)
    and the outer component will update after listening to these events.

    You can think of emitting events like putting out a flyer about an event. If someone is interested in this event, he or she can gather more information through reading the flyer.

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

<br>

5. **Computed properties**<br>
    This is useful when you want to compose new data based on the data that has changed.
    Instead of calling methods to do that whenever data has changed, computed properties will do it for you automatically.

    ```js
    computed: totalCount() {
        let result = 0;
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

6. **Watched properties**<br>
    Watched properties are used to call other functions when a particular data has been updated, such as <tooltip content="independent operations">asynchronous operations</tooltip>.

    For example, when a new `item` is added, we want to send a notification to our friend to alert him or her about the change.
    A watched property on `items` can be added so that a notification can be sent whenever `items` has changed.

    ```js
    watch: {
        totalCount: function() {
            let result = 0
            this.items.forEach((item) => result += item.count);
            this.totalCount = result;

            // notify friend about the change
        }
    }
    ```

    This may look quite similar to `Computed properties`.

    To decide which is more suitable for your feature, here is a brief comparison:
    Watched property | Computed property
    :-------------- | :----------------
    used for running expensive operations | used for updating data for dependencies
    executed every time page refreshes | uses cached data and executes only when changed
    **watches** for change in 1 property | **creates** a new property that is updated when 1 or more dependencies change

<br>

## Why use Vue?

Now that we know what Vue is, let us look at some benefits it has to offer.

### Benefit 1: Approachable

Vue is very easy to learn. Compared to other framework such as Angular and React, Vue is simple in term of API and design. Learning enough to build non-trivial applications typically takes less than a day. An example is provided below:<br/><br/>
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
    How is iteration like in Vue:<br/><br/>
    The JavaScript file in Vue
    ```js
    var Iteration = new Vue({
      el: '#array',
      data: {
        array: ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
      }
    });
    ```
    The HTML file in Vue
    ```html
    <div id="array">
      <span v-for="date in array">{date}</span>
    </div>
    ```

### Benefit 2: Progressive

Vue is designed from the ground up to be incrementally adoptable. The core library is focused on the view layer only, and is easy to pick up and integrate with other libraries or existing projects. This means that if you have a large application, you can plug Vue into just a part of your application without disturbing the other components. A quote from Evan You - the founder of VueJs is as follow:
    > Vue is a more flexible, less opinionated solution (than Angular). That allows you to structure your app the way you want it to be, instead of being forced to do everything the Angular way (Angular requires a certain way to structure an application, making it hard to introduce Angular into an already built project). It’s only an interface layer so you can use it as a light feature in pages instead of a full blown SPA (single-page application).
    >
    > -- [[source]](https://www.valuecoders.com/blog/technology-and-apps/vue-js-comparison-angular-react/)

### Benefit 3: Versatile
Vue is perfectly capable of powering sophisticated single-page applications when used in combination with modern tooling and supporting libraries.

### Benefit 4: Clean
Vue syntax is simple and this can make the HTML pages very clean. This would allow user interfaces built by Vue to be more maintainable and testable.

## Disadvantages of Vue

Like any other framework/library, Vue has its share of disadvantages.

1. **Relatively small size community:**<br/>
Vue is a relatively new JavaScript framework as compared to Angular and React. The size of the community for Vue is therefore relatively small. Although small size community means you can differentiate yourself from other JavaScript developers, it also means there are fewer resources such as tutorials and problem-shooting guides.

2. **Language barriers:**<br/>
A majority of users of Vue are the Chinese as Vue is developed by a Chinese American. He is supportive of the Chinese community and hence a lot of the existing plugins are written in Chinese. There might be some language barriers for an English speaking developer seeking for Vue resources.

## Resources

Detailed Comparison of Vue with other JavaScript frameworks can be found from:
- [Vue Guild: Comparison with Other Frameworks](https://vuejs.org/v2/guide/comparison.html)
- [Angular vs React vs Vue](https://medium.com/unicorn-supplies/angular-vs-react-vs-vue-a-2017-comparison-c5c52d620176)

Links to VueJs tutorials and practices:
- [VueJS Official Website](https://vuejs.org)
- [VueSchool](https://vuejs.org/)
- [Laracast](https://laracasts.com/series/learn-vue-2-step-by-step)
- [Vuetify](https://vuetifyjs.com/zh-Hans/)

</div>
