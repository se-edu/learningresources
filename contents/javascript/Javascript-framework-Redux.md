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

<!-- put table of contents here -->

## What is Redux?

<!-- understand what problem it solves -->
<!-- anedoctal example of a complex sitation that would then require react -->

Simply put, Redux is a library that manages application state for Javascript applications. You can use it together with view libraries such as React and Vue.

More formally, the [official website](https://redux.js.org/introduction/getting-started) describes Redux as follows:

> *Redux* is a predictable state container for JavaScript apps.
>
> It helps you write applications that behave consistently, run in different environments (client, server, and native), and are easy to test. On top of that, it provides a great developer experience, such as live code editing combined with a time traveling debugger.
>
> You can use Redux together with React, or with any other view library. It is tiny (2kB, including dependencies), but has a large ecosystem of addons available.

### What is application state? 

<!-- No need for this heading here, the explanation flows better. -->
<!-- or just footnotes -->

([Adapted from this Medium article](https://medium.com/javascript-in-plain-english/the-only-introduction-to-redux-and-react-redux-youll-ever-need-8ce5da9e53c6))

Think of it like a global object holding information that you will use for various purposes in your application. 

For example:
- For a general application you would need to keep store of whether a user is logged in and his/her user information
- For a to-do list you would need to know what items are currently in the list
- For a social media application you'd need to store serveral objects and arrays representing the stories to render in the News Feed

All of the above can be considered as state.

Modern web applications have large amount of state. And for complex apps, state is likely to be shared between components and updated from various parts of the code, making state less predictable. Which is why we need libraries like Redux to manage state. 

## How does Redux work?

<!-- shorten a simplify section as much as possible. no sub-headings. no code as much as possible -->
<!-- use diagram -->

### Store

The whole application state is stored in a **single** store, a plain object. For example, the state of a todo app might look like this:

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

There are no direct setters to the store. The store is read-only; the only way to change the store is by emitting an action, which we will learn about next. 

### Action

Something in the state can be changed by dispatching an **action**, a plain object.

```js
{ type: 'ADD_TODO', text: 'Go to swimming pool' }
{ type: 'TOGGLE_TODO', index: 1 }
{ type: 'SET_VISIBILITY_FILTER', filter: 'SHOW_ALL' }
```

### Reducer

The current state, and an action is passed into a **reducer**, a pure function which returns the next state of the application

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

These two reducers are called by a main reducer, and with that the complete app state is managed.

```js
function todoApp(state = {}, action) {
  return {
    todos: todos(state.todos, action),
    visibilityFilter: visibilityFilter(state.visibilityFilter, action)
  }
}
```

The reducers are **pure** functions. They take the previous state and an action, and return the next state, as new state objects, instead of mutating the previous state.

A careful eye would notice that the above code block examples on reducers are not pure functions. In fact, with some Redux API, a more proper example would then be the code block below. Don't worry if you don't fully understand the specific Javascript syntax and methods used (such as `.map` and the spread operator), but just understand that new state objects are returned when state is updated.

```js
function visibilityFilter(state = 'SHOW_ALL', action) {
  switch (action.type) {
    case 'SET_VISIBILITY_FILTER':
      return action.filter
    default:
      return state
  }
}
function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ]
    case 'COMPLETE_TODO':
      return state.map((todo, index) => {
        if (index === action.index) {
          return Object.assign({}, todo, {
            completed: true
          })
        }
        return todo
      })
    default:
      return state
  }
}
import { combineReducers, createStore } from 'redux'
const reducer = combineReducers({ visibilityFilter, todos })
const store = createStore(reducer)
```

# Redux Middlewares

# Redux Devtools

## Why use Redux?

<!-- Some points from this section were adapted from these article  box-->

([Adapted from this artcle](https://blog.logrocket.com/why-use-redux-reasons-with-clear-examples-d21bffd5835/))
([And this article](https://www.smashingmagazine.com/2016/06/an-introduction-to-redux/))

Now that we know what Redux is, let us take a look at some of the benefits that were made possible with these concepts.


<!-- confusing point, make simpler and tie to my article -->
1. **State is made predictable**<br/>
The features of Redux make state predictable: There is always one source of truth in the store. This store is immutable. Reducers are pure functions, always producing the same result for the same state and action. And with these features, it is possible to implement functionality that is otherwise difficult, such as Undo and Redo. <br/><br/> Because neither the view, nor the network callbacks directly write to state. And changes to state via actions are centralized and happen in strict order, there is no need to worry for race conditions in our applications

2. **Easier to debug and inspect**<br/>
Since actions and state are plain objects, it is possible to serialize them and log them. By logging them, it's easy to understand errors in code. You can also print the store within the code itself (for example using `console.log(store.getState())`{.js}). <br/><br/> The Redux DevTools is an amazing tool as well. Used in the browser, it provides features such as a tree visualization of the application state, as well a time travel ability to replay state changes. 

![Redux Devtool](https://user-images.githubusercontent.com/7957859/48663602-3aac4900-ea9b-11e8-921f-97059cbb599c.png)


<center>

_Redux DevTools_ <sup>[image source](https://user-images.githubusercontent.com/7957859/48663602-3aac4900-ea9b-11e8-921f-97059cbb599c.png)</sup>
</center>

3. **Easier testing**<br/>
It is easy make tests since the reducers used to change the state are pure functions. When writing code with Redux means writing small, pure and isolated reducer functions, we simply write small and isolated tests. Using Redux makes for testable code.

4. **Allows for server-side rendering**<br/>
Redux can be used for server-side rendering, by sending the store created on the server side in response to the client's server request. Information from the store then can be used for the initial render of the application. 

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

<!-- see ze yu's ocmment>

Given below are a few places that will help you ease into using Redux:

1. You can start with [The Redux official website](https://redux.js.org/). This article is based on the sections [Core Concepts](https://redux.js.org/introduction/core-concepts) and [Three Principles](https://redux.js.org/introduction/three-principles), so you may skim through those. The site has installation instructions and a tutorial.
1. Install the [Redux DevTools](https://github.com/reduxjs/redux-devtools)
1. If you're using React alongside Redux, read up on [React Redux](https://react-redux.js.org/), which are official React bindings for Redux
1. You can also checkout the [course](https://egghead.io/courses/building-react-applications-with-idiomatic-redux) by Redux's creator.
1. The official website has a well curated page on [Learning Resources](https://redux.js.org/introduction/learning-resources/)

</div>
