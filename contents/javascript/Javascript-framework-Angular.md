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

Author: [Ronak Lakhotia](https://github.com/RonakLakhotia)                                                              
Reviewers: [Tan Heng Yeow](https://github.com/tanhengyeow), [Jacob Li PengCheng](https://github.com/jacoblipech)

## What Is Angular?

Angular is one of the popular Javascript frameworks used for building web applications.
The framework aims to make development and testing of enterprise web applications easier. Angular is maintained by Google, as well as a community of developers.

>Angular started as a side project. Back in 2009, Miško Hevery and Adam Abrons released a project that would help developers build web applications using simple HTML tags. The name “Angular” comes from the angle brackets, or < >, that surround all HTML tags.
>
>Miško described the idea behind the framework in an <a href="https://www.infoworld.com/article/2612801/javascript/whats-so-special-about-googles-angularjs.html">interview done in 2013</a>. 

## Why learn Angular?

Angular is a complex framework and can be challenging for new developers to understand.
However, the advantages Angular provides justifies its steep learning curve.

1. **Angular supports Single Page Applications (SPAs).**

    <box>Single Page Applications are a type of web applications that load a single HTML page. The key feature is that SPAs can communicate with back-end servers without refreshing the full web page. SPAs load information on a single HTML page. The page updates dynamically through JavaScript as the user interacts with the app. This enhances the user experience.</box>
 
2. **Angular uses TypeScript.** Angular applications are built using TypeScript language, a superset of JavaScript. TypeScript provides static typing. This helps the compiler show warnings about any potential errors in the code, before the app runs. Another advantage is that TypeScript provides code completion using [Intellisense](https://docs.microsoft.com/en-us/visualstudio/ide/using-intellisense?view=vs-2017). IntelliSense provides active hints as code is added.

    <box> TypeScript was created and is maintained by Microsoft, and has been <a href="https://developer.telerik.com/featured/the-rise-of-typescript/">growing in popularity</a> for the last few years making it reliable to use.</box>
                                                  
3. **Google Long-Term Support.** Some software engineers consider the fact that Angular is supported by Google an advantage of the technology. While this may sound justified, it does not serve as much of an incentive. The good sign though is that Google announced Long-Term Support (LTS) for the technology back in 2017.

    These are some of the reasons why you might want to get familiar with Angular.

    [source](https://hackr.io/blog/why-should-you-learn-angular-in-2018)

### Benefit 1: Angular provides a declarative user interface.

Angular uses HTML to define the app’s user interface. HTML is a declarative language which is more intuitive than using imperative code to define the UI. An Imperative approach requires you to tell the compiler how you want the program to run by focussing on the logic implementation. Declarative code, on the other hand enables you to describe the logic without worrying about the implementation. How does it help? You do not need to invest time in dealing with how your program runs and deciding what to load first.

Lets see a working example. 

With jQuery you might write the following code to define a textbox that takes numeric values and doesn’t allow alphanumeric input or input that is beyond of a defined range.

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

<box type="info">
If you are not familiar with jQuery, you may want to get a brief understanding from the official <a href="https://jquery.com/">website</a>.
</box>

On the other hand, using versions Angular 2 and above, your code would look something like this:

```
<input numerictextbox k-min="-10" k-max="100" k-step="0.75"
  k-format="n" k-decimals="3"/>
```

Rather than spending time thinking on how the program should flow, you define what you want the user to see and Angular will take care of the dependencies.

The above example shows how Angular makes it easier for developers to design the UI elements.

[source](https://www.telerik.com/blogs/three-ds-of-web-development-1-declarative-vs-imperative)

### Benefit 2: Simplified MVC pattern and reduced coding.

Angular framework uses the [MVC](https://blog.angular-university.io/why-angular-angular-vs-jquery-a-beginner-friendly-explanation-on-the-advantages-of-angular-and-mvc/) (Model-View-Controller) structure. The HTML, is what is presented to the user. This represents the View component. Next to it, we have the `component.ts` files. This is the controller. Essentially, it can choose which data to push to our view (`.html`). 
Lastly, we have the model. In Angular, the model will mostly be our services, which we can access through our controller. Services are a way to retrieve, update and process data to and from the backend.

Most frameworks require developers to split the application into multiple MVC components. After that, the developer has to write code to put them together. However, Angular does not require you to connect these components together. As explained in this [article](https://blog.angular-university.io/why-angular-angular-vs-jquery-a-beginner-friendly-explanation-on-the-advantages-of-angular-and-mvc/) on the official website, everything happens under the hood. Angular ensures Separation of Concerns and automatic synchronization between the components. That saves developers time.

### Benefit 3: Angular CLI

In 2016 the Angular team announced the Angular CLI, a command-line interface (CLI) to automate your development workflow in applications using versions Angular 2 and above.
It is recommended to use Angular CLI for creating Angular apps as you don't need to spend time installing and configuring all the required dependencies.

> npm install -g @angular/cli

Running the above command installs the Angular CLI and gives you more control over your project.
With Angular CLI developers can generate Angular files, execute applications and even run end to end tests.

[source](https://www.amadousall.com/why-you-should-use-angular-cli/)

### What does an Angular app look like?

Let’s dive into some code written for Angular, starting with a little hello world example. The code is applicable for versions Angular 2 and above.

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

The one unique thing in the above example is the `<my-app>` element, as that’s not an element you would normally use in a web-based application. Angular extends the vocabulary of HTML by allowing you do define your own tags.
These custom tags are known as components.

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

In Angular we use the `@Component` tag to mark a class as an Angular component. It provides the metadata that determines how the component is processed during runtime.

* The use of `selector: my-app` is how Angular knows what to do when it sees a `<my-app>` tag in HTML.

* The `template` property controls what HTML gets rendered when this component is loaded.

A visual to summarize the above :

![Component](https://angular.io/generated/images/guide/architecture/component-databinding.png)

### When not to use Angular

Like any other framework/library, Angular has its share of disadvantages.

1. **Steep Learning curve** - Angular requires one to learn many concepts, such as directives, modules, decorators, components, services, dependency injection, pipes, templates and most importantly TypeScript. 
The sheer number of new concepts is confusing to newcomers.

2. **Opinionated Framework** - Angular enforces developers to follow a structure when designing the application. While libraries like React allow engineers the freedom to choose any third party library to integrate into their application.

    [source](https://www.altexsoft.com/blog/engineering/the-good-and-the-bad-of-angular-development/)
    
## What are the different versions of Angular?

There have been 6 versions of Angular so far. AngularJS, the first version, was released in 2010. Angular2, Angular4, Angular5, Angular6 and Angular7 are the more recent versions.
Each version has new enhancements as Google provides new updates in the framework periodically.

The differences between the various versions can be better understood from [this](https://www.simplilearn.com/angularjs-vs-angular-2-vs-angular-4-differences-article) article.    

### Angular when compared with other popular frontend frameworks

There are many frontend frameworks and libraries out there that developers may prefer over Angular. <a href="https://reactjs.org/">React</a>, a Javascript library, and <a href="https://vuejs.org/">Vue</a>, a Javascript framework, are two such examples.

Choosing a tech stack can become a tedious task as you need to take every factor into consideration including budget, time, app size, end-users, project objectives, and resources.

React is a popular Javascript library, open sourced by Facebook. The ease of learning React is a key advantage.
React provides more flexibility to developers as it allows integration of third party libraries. It also has a good set of developer tools that helps in debugging.
Similarly, Vue offers us some advantages, like flexibility, simplistic structure and ease of integration.

Every framework/library has its own pros and cons. The framework/library you choose depends on the requirements of the application. Below are a few resources to help you get a brief introduction to these frameworks/libraries.

1. [React vs Angular vs Vue](https://medium.com/@TechMagic/reactjs-vs-angular5-vs-vue-js-what-to-choose-in-2018-b91e028fa91d) - A brief comparison between the three most popular frontend tools.
2. [Why use Vue](https://medium.com/@brainmobi/advantages-of-using-vue-js-for-your-web-applications-7e460cadfffc) - Advantages of using Vue.
3. [Angular vs React](https://programmingwithmosh.com/react/react-vs-angular/) - A comprehensive comparison between React and Angular.

### How to get started with Angular?

1. The official [Angular](https://angular.io/guide/quickstart) website offers a good tutorial to get started with the new framework. This guide will show you how to build and run a simple Angular application.

2. This article on [freeCodeCamp](https://medium.freecodecamp.org/want-to-learn-angular-heres-our-free-33-part-course-by-dan-wahlin-fc2ff27ab451) is a good place to start writing simple applications using the features that Angular has to offer.

3. [Here](https://github.com/angular/angular) is a link to the official Angular framework maintained by Google. You could even open a PR and start contributing!

4. You should also start learning [TypeScript](https://www.typescriptlang.org/docs/home.html) and get comfortable with the new language.

[source](https://www.quora.com/What-are-the-different-versions-of-AngularJS)

### Where to Go from Here?

Angular is not the easiest framework to work with. However, it is reliable and is used by many developers to create large scale applications. The Angular community is big, and help is readily available via searching on Google, Stack Overflow, and all around the web.

There are many online resources and tutorials online that can help you ease into the Angular world.

- [Single Page Applications Explained](https://www.bloomreach.com/en/blog/2018/07/what-is-a-single-page-application.html)
- [Angular Components - Ten Basic Concepts](https://angularfirebase.com/lessons/angular-components-basics-top-ten/) - A QuickStart on Angular's Component driven architecture.
- [StackOverflow Questions](https://stackoverflow.com/questions/tagged/angular?sort=votes&pageSize=50) - Top voted questions on Angular
- [Angular in depth](https://blog.angularindepth.com/) - Advanced concepts in Angular explained

</div>
