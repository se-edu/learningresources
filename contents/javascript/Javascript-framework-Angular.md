<frontmatter>
  title: Introduction to Angular
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Introduction to Angular

Author(s): [Ronak Lakhotia](https://github.com/RonakLakhotia)

## What Is Angular?

Angular is an open-source frontend Javascript framework that helps you build <a href="https://www.bloomreach.com/en/blog/2018/07/what-is-a-single-page-application.html"> Single Page Web Applications</a>.
The library provides a number of features that make it trivial to implement the complex requirements of modern applications, such as data binding, routing, and animations. Angular is maintained by Google, as well as a community of individual developers

>Angular started as a side project. Back in 2009, Miško Hevery and Adam Abrons released a project called <angular /> that would help developers, as well as designers, build web applications using simple HTML tags. The name “Angular” comes from the angle brackets, or < >, that surround all HTML tags.
>
>Miško described the idea behind the framework in an <a href="https://www.infoworld.com/article/2612801/javascript/whats-so-special-about-googles-angularjs.html">interview done in 2013</a>. 

With Angular, designers can use HTML as the template language and it allows for the extension of HTML's syntax to convey the application's components effortlessly. Angular makes much of the code you would otherwise have to write, completely redundant.

## What are the different versions of Angular?

AngularJS or Angular1 is a JavaScript front-end framework developed by Google whereas Angular is a TypeScript-based open-source front-end web application platform led by the Angular Team at Google.
There have been 5 versions of Angular so far, which are AngularJS, Angular2, Angular4, Angular5 and the latest one being Angular6.
There are key differences between the various versions as Google tried to enhance the tool given the fierce competition from React and Vue. 
More details about the differences can be found in the this [Quora](https://www.quora.com/What-are-the-different-versions-of-AngularJS) post.



## Why learn Angular?

Angular is not the world’s simplest framework, and it takes time to understand some of the concepts that Angular has to offer.
But once you do, there are some really powerful things you can do in a surprisingly small amount of code.

1. **Angular supports Single Page Applications.** Single Page Applications are a type of web applications that load a single HTML page, and the page is updated dynamically according to the interaction of the user. SPAs can communicate with the back-end servers without refreshing the full web page, for loading data in the application. They provide better user experience and also eases the development process.
                                                  The fact that Angular supports the development of SPAs, makes it worth learning!
 
2. **Angular uses TypeScript.** Angular applications are built using TypeScript language, a superscript for JavaScript, which ensures higher security as it supports types (primitives, interfaces, etc.). It helps catch and eliminate errors early when writing the code or performing maintenance tasks. This language ensures improved navigation, refactoring, and autocompletion services thereby easing the development process for engineers. You can even opt out of its inbuilt features when needed.
TypeScript was created and is maintained by Microsoft, and has been <a href="https://developer.telerik.com/featured/the-rise-of-typescript/">growing in popularity</a> for the last few years; therefore it’s not much of a risk to incorporate TypeScript in to your application development. It’s not going anywhere.
                                                  
3. **Google Long-Term Support.** Some software engineers consider the mere fact that Angular is supported by Google a major advantage of the technology. While this may sound justified, Google itself is not enough. The good sign though is that Google announced Long-Term Support (LTS) for the technology. Igor Minar and Steven Fuin, the engineers behind Angular, confirmed this commitment in the <a href="https://medium.com/google-developer-experts/my-picks-from-ng-conf-2017-d971842c0d05">ng-conf 2017 Keynote</a>. 

### Benefit: Angular provides a declarative user interface.

Angular uses HTML to define the app’s user interface. HTML is a declarative language which is more instinctive and less convoluted than defining the interface procedurally in JavaScript. How does it help? You don’t need to invest your time in program flows and in deciding what loads first. Define what you require and Angular will take care of it. Plus you can bring in many more UI developers when the opinion is written in HTML.
Directives are Angular’s way of bringing additional functionality to HTML. In other words, declarative code enables you to describe what you want done without worrying about how.

Lets see a working example. 

With jQuery you might write the following code to define a textbox that takes numeric values and doesn’t allow alphanumeric input or input that is outside of a defined range.

```
$("#textbox").numericTextBox({
    value: 10,
    min: -10,
    max: 100,
    step: 0.75,
    format: "n",
    decimals: 3
});
```

In the code above, jQuery was instructed to find a DOM element with an id of textbox, then call the extension method numericTextBox and pass in the various parameters that configure the initial value, ranges, etc.

On the other hand, using Angular4, your code would look something like this:
```
<input numerictextbox k-min="-10" k-max="100" k-step="0.75"
  k-format="n" k-decimals="3"/>
```
Instead of worrying about how to transform the element, you simply declare what you want to happen. It should be a numeric textbox; it should have a minimum value of -10; etc.
Rather than spending time on how the program flows and what should get loaded first, you can simply define what you want and Angular will take care of the dependencies.


### Benefit: Simplified MVC pattern and reduced coding.

Angular framework is embedded with original MVC (Model-View-Controller) software architectural setup. However, it is not according to the established standards. Angular does not ask developers to split an application into different MVC components and build a code that could unite them.

![MVC Pattern](https://www.grazitti.com/assets/2018/06/reasons-5.jpg)

Rather, it only asks to divide the app and takes care of everything else. Therefore, Angular and MVVM (Model-View-View-Model) design structure are quite similar. Angular ensures easy development as it eliminates the need for unnecessary code. It has a simplified MVC architecture, which makes writing getters and setters needless.
All in all, developers are promised less coding, along with lighter and faster apps. This is very important for enterprise web applications.
>According to a <a href="https://blog.qburst.com/2017/05/make-your-apps-load-faster-with-angular-2/">survey</a>, every 100-millisecond improvement in page loading speed led to 1% increase in revenue for Amazon.


### What does an Angular app look like?

Let’s dive into some code, starting with a little hello world example. All Angular apps start in HTML.

```
<!doctype html>
<html>
  <head>
    <!-- The various &lt;script&gt; tags needed to load Angular -->
    <!-- For a full list, see https://angular.io/docs/ts/latest/quickstart.html -->
  </head>
  <body>
    <my-app></my-app>
  </body>
</html>
```

The one unique thing in the above example is the `<my-app>` element, as that’s not an element you’d regularly use in a web-based application. Angular’s goal has always been to extend the vocabulary of HTML, and it does so by allowing you do define your own tags.
These custom tags are known as components and you define their behavior in code. Here’s the world’s simplest implementation of the `<my-app>` element.

Let's now look at some TypeScript code that defines the `<my-app>` component.

```
import { Component } from "@angular/core";

@Component({
  selector: "my-app",
  template: `
    <h1>Hello World</h1>
  `
})
export class AppComponent {}
```

In Angular we use the `@Component` tag, which is known as a decorator, to mark classes that should be considered elements that can be used in your HTML markup. You have the ability to pass that `@Component` properties to describe the element.

* The selector property determines that tag’s name when typed in HTML. The use of selector: "my-app" is how Angular knows what to do when it see a `<my-app>` tag in HTML.

* The template property controls what HTML gets rendered when this component is used. The use of template: `<h1>Hello World</h1>` is how Angular determines what to render when it sees `<my-app>`, and is why this app renders a basic `<h1>` tag when you preview it in a web browser.

A visual to summarize the above :

![Component](https://angular.io/generated/images/guide/architecture/component-databinding.png)

### Why not to use Angular

Like any other framework/library, Angular has its share of disadvantages.

1. **Steep Learning curve** - Angular has many topics to learn, starting from basic ones such as directives, modules, decorators, components, services, dependency injection, pipes, templates and most importantly TypeScript. 
The sheer number of new concepts is confusing to newcomers. And even after you’ve started, the experience might be a bit rough since you need to keep in mind things like Rx.js subscription management and change detection performance.

2. **Opiniated Framework** - Angular tells developers how to structure their application. While libraries like React allow engineers the freedom to choose any third party library to integrate into their application.

3. **Performance** - In the past, Dynamic applications written in Angular did not always perform that well even though improvements have been made recently. Complex SPAs could be laggy and inconvenient to use due to their size. 

### Angular when compared with other popular frontend frameworks


There are many frontend frameworks out there that many developers prefer over Angular. Two of the most popular tools are <a href="https://reactjs.org/">React</a> and <a href="https://vuejs.org/">Vue</a>.

Choosing a tech stack sometimes becomes a tedious task as you need to take every factor into consideration including budget, time, app size, end-users, project objectives, and resources.

Angular is opinionated and tells developers how to structure their code. It greatly eases the engineering process, especially in large teams. However the steep learning curve, amongst other disadvantages,
make developers adopt other tools.

React is a popular Javascript framework, open sourced by Facebook. The ease of learning React is a key advantage that it has.
If your project involves many components with different, often changing states , dynamic inputs, user login and access permissions – then the project may be a good fit for React. React helps you manage those changing states and dynamically present different views to the user based on state information.
This <a href="https://www.intertech.com/Blog/react-tutorial-when-to-use-react-and-why/">article</a> lists the benefits of using React. 
Similarly, Vue offers us some advantages, like flexibility, simplistic structure and ease of integration.

Every framework has its own pros and cons, meaning it all depends on the developers skills, requirements of the application and many factors like scalability, maintenance, familiarity etc. There are many great tools for developers to choose. Below are a few resources to help you get a brief introduction to these frameworks/libraries.

1. [React vs Angular vs Vue](https://medium.com/@TechMagic/reactjs-vs-angular5-vs-vue-js-what-to-choose-in-2018-b91e028fa91d) - A brief comparison between the three most popular frontend frameworks
2. [Why use Vue](https://medium.com/@brainmobi/advantages-of-using-vue-js-for-your-web-applications-7e460cadfffc) - Advantages of using Vue
3. [Angular vs React](https://programmingwithmosh.com/react/react-vs-angular/) - A comprehensive comparison between React and Angular

### How to get started with Angular?

1. The official [Angular](https://angular.io/guide/quickstart) website offers a good tutorial to get started with the new framework. This guide will show you how to build and run a simple Angular application.

2. This article on [freeCodeCamp](https://medium.freecodecamp.org/want-to-learn-angular-heres-our-free-33-part-course-by-dan-wahlin-fc2ff27ab451) is a good place to start writing simple applications using the features that Angular has to offer.

3. [Here](https://github.com/angular/angular) is a link to the official Angular framework maintained by Google. You could even open a PR and start contributing!

4. You should also start learning [TypeScript](https://www.typescriptlang.org/docs/home.html) and get comfortable with the new language.

### Where to Go from Here?

With Angular you’re not getting the world’s easiest-to-use framework, but you are getting an incredibly robust, well-documented, reliable and effective framework used by millions of developers to create powerful applications. The Angular community is enormous, and help is readily available via Google searches, Stack Overflow, and all around the web.

There are many online resources and tutorials online that can help you ease into the Angular world.

- [Angular Components - Ten Basic Concepts](https://angularfirebase.com/lessons/angular-components-basics-top-ten/) - A QuickStart on Angular's Component driven architecture.
- [StackOverflow Questions](https://stackoverflow.com/questions/tagged/angular?sort=votes&pageSize=50) - Top voted questions on Angular
- [Angular in depth](https://blog.angularindepth.com/) - Advanced concepts in Angular explained

</div>
