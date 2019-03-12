<frontmatter>
  title: React
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# React

Author: [Aadyaa Maddi](https://github.com/amad-person)

### What is React?

**React** is an open-source JavaScript <tooltip content="React only handles the view component of your application. It is not an MVC framework!">library</tooltip> for building composable user interfaces. It was developed by Facebook and released to the world in 2013, and is now used in a huge number of popular applications, including Facebook and Instagram.

<box type="tip">
    If you have the <a href="https://github.com/facebook/react-devtools">React DevTools</a> extension installed in your browser, you can inspect the webpage of either of these applications and observe that a lot of React components are being used by them! 
</box>

React abstracts away the [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction) from you, giving you a simpler programming model and better performance.
It can also [render on the server using Node](https://alligator.io/react/server-side-rendering/), and it can power native mobile applications using [React Native](https://facebook.github.io/react-native/).

### Why React?

React allows developers to create large web applications which can change data, without reloading the page. The main purpose of React is to be fast, scalable, and simple. 
Some of its benefits are described in detail below.

#### React is Declarative

With React's <tooltip content="Declarative programming focuses on what the program should accomplish without specifying how the program should achieve the result.">declarative</tooltip> approach to building user interfaces, you can create interaction-rich applications. 
You can build these interfaces without manipulating the DOM directly, and even have an event system that doesn't require you to interact with actual DOM events. 

This is unlike the traditional <tooltip content="Imperative programming focuses on explicitly describing how a program operates.">imperative</tooltip> approach of building applications. An example would be looking up elements in the DOM using [jQuery](https://jquery.com/), where you have to tell the browser what to do and how to update the elements whenever data changes.  

You can see React's declarative approach in action in the sandbox below:

<iframe src="https://codesandbox.io/embed/p564px0j47?fontsize=12" style="width:100%; height:400px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

<box type="info">
    The sandbox above is editable. You can play around with the application `state` and see how the view automatically gets updated. 
</box>

Notice that with React, you just need to specify *what* you want to do when the application data changes. React will efficiently update and render just the right views without you having to interact with the DOM. 

#### React is Component-Based

In the example above, you might have noticed that the application extends a `Component` class. This brings us to the next benefit of React - it is component-based. 

A component is just a JavaScript function that accepts arbitrary input (called `props`) and returns elements describing what should appear on the screen. You can think of a component as an encapsulated piece of your interface that manages its own data and logic. 

Multiple components can be used to compose complex user interfaces. For example, if you have a todo list application, it could have a `Form` component to handle adding new items, and a `List` component to display the items.
Furthermore, the `List` component could be composed of multiple `ListItem` components, one for each item in the todo list. 

The sandbox below has a simple example to show you how components work:

<box type="info">
    Notice the HTML-like syntax within the JavaScript method <code>render()</code>. This is <a href="https://reactjs.org/docs/introducing-jsx.html">JSX</a>, and it is a JavaScript syntax extension developed by the React team.
</box>

<iframe src="https://codesandbox.io/embed/884m88wr90?fontsize=12" style="width:100%; height:400px; border:0; border-radius: 4px; overflow:hidden;" sandbox="allow-modals allow-forms allow-popups allow-scripts allow-same-origin"></iframe>

<br/>

With components written in JavaScript, you can easily pass data through your application and keep state out of the DOM. You can read more about components and props in the React [documentation](https://reactjs.org/docs/components-and-props.html). 

#### React is Fast

Web applications can have a lot of user interaction and data updates. Extensive DOM manipulation can be a performance bottleneck in these applications as the DOM is tree-structured - simple changes at the top can trigger the entire application to get updated. 

React solves this problem by using a virtual DOM. The virtual DOM is a JavaScript object that is kept in the memory of your application. In other words, it is a virtual representation of the actual DOM.

Any updates to your application will first be made to the virtual DOM. Then, React will compare the virtual DOM with the actual DOM using a [diffing algorithm](https://reactjs.org/docs/reconciliation.html) to find out what changes have to be made to the actual DOM. 
Finally, React figures out how to update the actual DOM in the most effective way possible to guarantee minimum update time. This results in better performance and a cleaner user experience, even with data-intensive applications.

Those are the three main benefits of React. Other advantages are as follows:
 - React only allows data to flow downwards (one-way data binding), which makes your application easier to debug.
 - It is a constantly developing open-source library that is backed by Facebook, so you can be sure of getting community support.
 - If your application is large, you can manage its state conveniently using [Redux](https://redux.js.org/introduction/getting-started). Note that Redux can be used with any other view library, but it is mostly used with React. Hence, there are more resources for learning React + Redux.

### Why not React?

Like any other library, React has its disadvantages, which are as follows:
 - The high pace of development means that you would need to regularly relearn how to do things.
 - The huge number of tools you can integrate with React don't always have comprehensive documentation.
 - React is just a UI library. As React only allows one-way data binding, you would need to use [Flux](https://github.com/facebook/flux), a new application architecture introduced by Facebook that favours unidirectional data flow. 
 However, if you prefer working with the MVC pattern, you should consider a framework that lets you use it.

#### React and other Competing Alternatives

There are a lot of JavaScript frameworks and libraries that you can use to build your next web application. Some popular alternatives to React are [Angular](https://angular.io/) and [Vue](https://vuejs.org/).

How do you decide which one to use? Here are some resources to help you choose between them:
- [React, Angular, Vue: What they can do and which one is for you](https://blog.teamtreehouse.com/react-angular-vue) - guidelines for choosing which technology to learn.
- [Angular vs Vue vs React](https://www.codeinwp.com/blog/angular-vs-vue-vs-react/) - in addition to comparing the three technologies, this article aims to give a general structure for comparing JavaScript frameworks and libraries. Hence, you can use this structure to choose between any new frameworks that may arrive in the future.
- [State of JS 2018: Front-end Frameworks](https://2018.stateofjs.com/front-end-frameworks/overview/) - survey comparing the average salaries, company size, developer satisfaction, etc. for the most used JavaScript front-end technologies as of 2018.

React is best suited for projects that have a lot of user interactions that manipulate the DOM extensively. 
You should choose React if you want the flexibility to choose your application's stack, as it is not opinionated.
Additionally, if you need a mobile version for your app, React Native will make the development process considerably faster as most of the React code in your web version can be reused. 

However, if you prefer out-of-the-box solutions with a strong application structure, you should consider other alternatives like Angular or Vue. Additionally, you should prefer these alternatives if you want to follow
the traditional MVC framework, as integrating React into an MVC framework will require additional configurations.

Every framework has its pros and cons, but hopefully you have managed to see that React removes some of the complexity that comes with building user interfaces (see [this](https://jlongster.com/Removing-User-Interface-Complexity,-or-Why-React-is-Awesome) blog post for a more elaborate explanation). 

### How to get started with React?

The official React [website](https://reactjs.org/) is a great place to get started. It includes:
 - A step-by-step [tutorial](https://reactjs.org/tutorial/tutorial.html) for building a React application, if you prefer to learn by doing.
 - A handy [guide](https://reactjs.org/docs/hello-world.html) to master the main concepts of React, if you prefer to learn by reading instead.
 - An [API reference](https://reactjs.org/docs/react-api.html) if you are familiar with React already.
 
If you want to add React to an existing project, you can take a look at React's official [guide](https://reactjs.org/docs/add-react-to-a-website.html) for doing so.
Alternatively, if you are creating a new React application, you can use one of the [recommended toolchains](https://reactjs.org/docs/create-a-new-react-app.html) to get the best user and developer experience. 
[Create React App](https://github.com/facebook/create-react-app) is a comfortable environment for learning React, and it is the best way to create <tooltip content="A single-page application is an app that works inside a browser and does not require page reloading during use.">single-page applications</tooltip> with React.

The official website also has advanced guides if you want to understand how React works behind the scenes.

### Where to go from here?

As React is a fairly popular library, you can find a lot of comprehensive resources online. Here are some resources to get started:
 - [The React Handbook](https://medium.freecodecamp.org/the-react-handbook-b71c27b0a795) - a well-rounded overview of React.
 - A list of officially [recommended courses](https://reactjs.org/community/courses.html) (some of which are free) - if you prefer third-party books or video tutorials. 
 - The React [blog](https://reactjs.org/blog/) - updates about React's latest features will be available here.
 - A list of [project ideas](https://flaviocopes.com/sample-app-ideas/) to build using React - if you prefer learning by building simple applications.  

React also has a [community](https://reactjs.org/community/support.html) of millions of developers that are active on [Stack Overflow](https://stackoverflow.com/questions/tagged/reactjs)
and discussion forums like [Dev](https://dev.to/t/react) and [Hashnode](https://hashnode.com/n/reactjs).

Lastly, if you want to know what to learn after getting familiar with React, here is a comprehensive [roadmap](https://github.com/adam-golab/react-developer-roadmap) that you can follow to become a full-fledged React developer.

</div>
