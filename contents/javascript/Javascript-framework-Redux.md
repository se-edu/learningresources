<frontmatter>
  title: Redux
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">
  
{{ booktitle | safe }}

# Introduction to Redux

**Author: [Labayna Neil Brian Narido](https://github.com/nbriannl)** <br>
Reviewers: 

<!-- update table of contents here -->
<box id="article-toc">

* [Introduction to Go‎](#introduction-to-go)
    * [What Is Go?‎](#what-is-go)
    * [Why Learn Go?](#why-learn-go)
        * [Benefit: Better Variable Declarations](#benefit-better-variable-declarations)
        * [Benefit: Better Support for Concurrency‎](#benefit-better-support-for-concurrency)
        * [Benefit: Better Error Handling](#benefit-better-error-handling)‎
        * [Benefit: `defer` Execution‎](#benefit-defer-execution)
        * [Benefit: Good Support for Interfaces](#benefit-good-support-for-interfaces)
        * [Benefit: Canonical Coding Style‎](#benefit-canonical-coding-style)
    * [How to Get Started with Go?‎](#how-to-get-started-with-go)
        * [Where to Go from Here?‎](#where-to-go-from-here)
</box>

## What is Redux?

<!-- understand what problem it solves -->
<!-- anedoctal example of a complex sitation that would then require react -->
To understand what Redux is, we have to understand what application state is.

You can think of application state like a global object holding information that you will use for various purposes in your application. 

For example:
- For a general application you would need to keep store of whether a user is logged in and his/her user information
- For a to-do list you would need to know what items are currently in the list
- For a social media application you'd need to store serveral objects and arrays representing the stories to render in the News Feed

All of the above can be considered as application state.

However, modern web applications have large amount of state. And for complex apps, state is likely to be shared between components and updated from various parts of the code, making state less predictable. We need a solution to manage a state that has become larger, and less predictable. Which is where Redux comes into the picture. 

**Redux** is a library that manages application state for Javascript applications. You can use it together with view libraries such as React and Vue.

More formally, the [official website](https://redux.js.org/introduction/getting-started) describes Redux as follows:

> *Redux* is a predictable state container for JavaScript apps.
>
> It helps you write applications that behave consistently, run in different environments (client, server, and native), and are easy to test. On top of that, it provides a great developer experience, such as live code editing combined with a time traveling debugger.
>
> You can use Redux together with React, or with any other view library. It is tiny (2kB, including dependencies), but has a large ecosystem of addons available.

<!-- No need for this heading here, the explanation flows better. -->
<!-- or just footnotes -->

<!-- ([Adapted from this Medium article](https://medium.com/javascript-in-plain-english/the-only-introduction-to-redux-and-react-redux-youll-ever-need-8ce5da9e53c6)) -->

## How does Redux work?

Now then, what is it about Redux that solves this?

<!-- shorten a simplify section as much as possible. no sub-headings. no code as much as possible -->
<!-- use diagram -->

The structure of Redux is relatively simple.

![Structure of Redux](javascript-framework-redux-images/3basiccomponents.png "Structure of Redux")

<center>

_The three building blocks of Redux — Actions, Reducer and Store_
</center>

For this section, let's think of a simple to-do list application.

The whole application state is stored in a **single** store, a plain object.

```js
{
  todos: [{
    text: 'Eat food',
    completed: true
  }, {
    text: 'Exercise',
    completed: false
  }],
  visibilityFilter: 'SHOW_COMPLETED'
}
```

There are no direct setters to the store. The store is read-only. Information in the store is used for various application functionality, such as the App UI. For example, based on the array under `todos`, we can render a to-do list of tasks. Based on the `visibilityFilter`, the application knows whether to render incompleted items. 

The only way to change something in the state is by dispatching an **action**, a plain object.

```js
{ type: 'ADD_TODO', text: 'Go to swimming pool' }
{ type: 'TOGGLE_TODO', index: 1 }
{ type: 'SET_VISIBILITY_FILTER', filter: 'SHOW_ALL' }
```

User interactions in the application, such as button click events, can correspond to actions. For example, in our to-do list application, filling up 'Go to swimming pool' and clicking a 'Add To-do' button would correspond to a `ADD_TODO` action. 

The current state, and an action is passed into a **reducer**, a function which returns the next state of the application

In a real-world application, instead of a single reducer which can get complex, we can have multiple, simpler reducers, and combine them into one reducer.

For example, we have two simpler reducers.

Firstly, a reducer for the visibility filter `visibilityFilter`:

```js
function visibilityFilter(state = 'SHOW_ALL', action) {
  if (action.type === 'SET_VISIBILITY_FILTER') {
    return action.filter
  } else {
    return state
  }
}
```

And secondly, a reducer, that manages the todo-list items `todos`:

```js
function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([{ text: action.text, completed: false }])
    case 'TOGGLE_TODO':
      return state.map((todo, index) =>
        action.index === index
          ? { text: todo.text, completed: !todo.completed }
          : todo
      )
    default:
      return state
  }
}
```

These two reducers are called by a main reducer, and with that the complete application state is managed.

```js
function todoApp(state = {}, action) {
  return {
    todos: todos(state.todos, action),
    visibilityFilter: visibilityFilter(state.visibilityFilter, action)
  }
}
```

With the updated application state, the application will reflect the new information in the store accordingly, for example, it may render new items, or render incomplete items instead of complete items. 

The reducers are <tooltip content="A pure function is a function which: Given the same input, will always return the same output. And produces no side effects.">**pure functions**</tooltip>. They take the previous state and an action, and return the next state as new state objects, instead of mutating the previous state.

A careful eye would notice that the above code block examples on reducers are not pure functions. If you're interested, you may see how the reducer should actually be written in the [official Redux site](https://redux.js.org/basics/reducers/#source-code). The code in the link includes some Redux API, and Javascript methods such as [`.map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) and the [spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) to make the functions pure.

## Why use Redux?

<box type='definition' header="Some points given in this section were adapted from these articles">

* [Why use Redux? Reasons with clear examples by Neo Ighodaro](https://blog.logrocket.com/why-use-redux-reasons-with-clear-examples-d21bffd5835/)
* [Redux · An Introduction by Alex Bachuk](https://www.smashingmagazine.com/2016/06/an-introduction-to-redux/)
</box>

Now that we know what Redux is, let us take a look at how the structure of Redux and its concepts make possible some benefits.

<!-- confusing point, make simpler and tie to my article -->
1. **State is made predictable**<br/>
In Redux, the single immutable store is the single source of truth for application state. Neither the view of the application, nor network callbacks directly write to it. Changes to the state are via actions, which Redux centralizes and keep in strict order. Also, reducers are pure functions, always producing the same result for the same state and action. Because of all of this, there is a structured and predictable cycle of which applicable state is updated, making state is predictable. Having such a a predictable state, makes it possible to implement functionality that is otherwise difficult, such as Undo and Redo.

2. **Easier to debug and inspect**<br/>
Since actions and state are plain objects, it is possible to serialize them and log them. In Redux, especially with the Redux DevTools, you can view a log of actions that took place, making it easy to understand errors in code. You can also print the store <tooltip content="for example using `console.log(store.getState())`">within the code itself</tooltip>. The Redux DevTools, used in the browser, also provides features such as a tree visualization of the application state, as well a time travel ability to replay state changes.

![Redux Devtool](https://user-images.githubusercontent.com/7957859/48663602-3aac4900-ea9b-11e8-921f-97059cbb599c.png)

<center>

_Redux DevTools_ <sup>[image source](https://user-images.githubusercontent.com/7957859/48663602-3aac4900-ea9b-11e8-921f-97059cbb599c.png)</sup>
</center>

3. **Easier testing**<br/>
Using Redux makes for testable code. Since the reducers used to change the state are pure functions, it is easy make tests. Writing small, pure and isolated reducer functions in Redux, means we write small and isolated tests. 

4. **Allows for server-side rendering**<br/>
Redux is one way to fulfill server-side rendering. This can be acheived by sending the store created on the server side in response to the client's server request. Information from the store then can be used for the initial render of the application.

## Why not to use Redux?

While Redux has its benefits, you shouldn't jump to Redux for every application you make.

<!-- make framework agnostic -->
As Dan Abramov, one of the creators of Redux, says:

<box light>
I would like to amend this: don't use Redux until you have problems with vanilla React.
</box>

 But the general idea on when to use Redux is when:

1. Reasonable amount of data changing over time
1. You need a single source of truth
1. Keeping everything in a top-level parent component is not enough

There are many sites and articles speaking of why you shouldn't or when you shouldn't use Redux. If you are interested, the following resources provide more information and thoughts on when Redux is meant to be used:

<!-- shift to getting start section -->
- [You Might Not Need Redux](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367)
- [When should I use Redux?](https://redux.js.org/faq/general#when-should-i-use-redux)

## Getting Started With Redux

<!-- see ze yu's ocmment-->

Given below are a few places that will help you ease into using Redux:

1. You can start with [The Redux official website](https://redux.js.org/). This article is based on the sections [Core Concepts](https://redux.js.org/introduction/core-concepts) and [Three Principles](https://redux.js.org/introduction/three-principles), so you may skim through those. The site has installation instructions and a tutorial.
1. Install the [Redux DevTools](https://github.com/reduxjs/redux-devtools)
1. If you're using React alongside Redux, read up on [React Redux](https://react-redux.js.org/), which are official React bindings for Redux
1. You can also checkout the [course](https://egghead.io/courses/building-react-applications-with-idiomatic-redux) by Redux's creator.
1. The official website has a well curated page on [Learning Resources](https://redux.js.org/introduction/learning-resources/)

</div>
