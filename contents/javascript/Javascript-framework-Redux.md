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

## What is Redux

Simply put, Redux is library that manages application states for Javascript applications. You can use it together with view libraries such as React or Angular.

More formally, the [official website](https://redux.js.org/introduction/getting-started) describes Redux as follows:

> *Redux* is a predictable state container for JavaScript apps.
>
> It helps you write applications that behave consistently, run in different environments (client, server, and native), and are easy to test. On top of that, it provides a great developer experience, such as live code editing combined with a time traveling debugger.
>
> You can use Redux together with React, or with any other view library. It is tiny (2kB, including dependencies), but has a large ecosystem of addons available.

### What is application state

([Adapted from this Medium article](https://medium.com/javascript-in-plain-english/the-only-introduction-to-redux-and-react-redux-youll-ever-need-8ce5da9e53c6))

Think of it like a global object holding information that you will use for various purposes in your application. 

For example:
- For a general application you would need to keep store of whether a user is logged in and his/her user information. 
- For a to-do list you would need to know what items are currently in the list
- For a social media application you'd need to store serveral objects and arrays representing the stories the to render in the News Feed. 

All of the above can be considered as state.

Modern web applications have massive state. Which is why we need libraries like Redux to manage state.

## How does Redux work?

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

There are no direct setters to the store. The store is read-only, the only way to change the store is by emitting an action, which we will learn about next. 

### Action

Something in the state can be changed by dispatching an **action**, a plain object.

```js
{ type: 'ADD_TODO', text: 'Go to swimming pool' }
{ type: 'TOGGLE_TODO', index: 1 }
{ type: 'SET_VISIBILITY_FILTER', filter: 'SHOW_ALL' }
```

### Reducer

The current state, and an action is passed into a **reducer**, a pure function which returns the next state of the application

In real-world application, instead of one complex reducer, we have multiple simpler reducers and combine them into one reducer.

For example, we two simpler reducers, firstly, a reducer for the visibility filter `visibilityFilter`:

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

These two reducers are called by a main reducer, that esssentially manages the complete app state.

```js
function todoApp(state = {}, action) {
  return {
    todos: todos(state.todos, action),
    visibilityFilter: visibilityFilter(state.visibilityFilter, action)
  }
}
```

The reducers are pure functions. They take the previous state and an action and return the next state, as new state objects, instead of mutating the previous state.

A careful eye would notice that the above code block examples on reducer the are not pure function. In fact, with some Redux API, a more proper example for reducer(s) would be:

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

Now you know what Redux is!

### UI

## Why Redux

### Easier debug and inspect
#### Redux dev tools

### Trivial implementation of functionality

### Something about no worry about race conditions

## Why Not Redux

There are many sites and articles speaking of why you shouldn't or when you shouldn't use Redux.

Most of the main ideas are
1. Reasonable amount of data changing over time
1. You need a single source of truth
1. Keeping everything in a top-level parent component is not enough

### Alternative state managers

## Get Started 

## Where to go from here

</div>
