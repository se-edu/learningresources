<frontmatter>
  title: Introduction to Ruby on Rails
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Introduction to Ruby on Rails

Authors: [Chattoraj Ayush](https://github.com/AyushChatto)

## Ruby on Rails overview

**Ruby on Rails** is an **easy-to-learn** Ruby framework that allows you to develop powerful web applications **quickly** and with as little effort as possible. It was developed in the early 2000s by Danish Programmer David Heinemeier Hansson, or 'DHH'. 

Ruby on Rails, or simply Rails, gained widespread popularity in the 2000s, and continues to be one of the most popular web-frameworks even to this day, because of how quickly it allows an easily scalable minimum viable product to be built, making it the perfect choice for startups. This is owed in large part to Rails being an **opinionated** framework. Rails believes that there is a 'right' way to do things (what they refer to as the 'Rails' way) - and while this means that it is significantly less flexible than some other frameworks, it also means that the amount of work someone has to do to make a website is much less, since Rails will 'automagically' fill in much of the boilerplate code. As DHH put it:-  

> ... One of the early productivity mottos of Rails went: “You’re not a beautiful and unique snowflake”. It postulated that by giving up vain individuality, you can leapfrog the toils of mundane decisions, and make faster progress in areas that really matter.
>
> *David Heinemeier Hansson,* __Convention Over Configuration, The Rails Doctrine__


### M-V-C Framework 
The 'Rails' way also encourages you to adopt an M-V-C framework in your applications - i.e. Model-View-Controller framework, and all projects default to using an MVC framework. The structure of apps following an MVC framework is as follows:-

<center>
<img src="MVC-Process.svg">
_Figure 1. Model-View-Controller Framework_ <sup>[source](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller)</sup>
</center>

In a nutshell, the View is what the User sees. These are, by default, .erb, or Embedded RuBy files, which are just HTML files with lines of Ruby code injected in places, to allow the page elements to manipulated by actions. A Model is basically just a Class (à la Object-Oriented Programming), that can be instantiated and made to encapsulate the values needed for the application. The Model is also referred to as the `Active Record` of a Rails Project, as it is an implementation of the Active Record Pattern, where the Model is mapped to the database, and instead of interacting directly with the database, you can simply interact with the Model in the language of the application, in this case, Ruby. The Controller acts as an interface between the View and the Model, and decides what actions to take and what page to render based on any input. 

For every HTTP request to a Rails application, the `routes.rb` file will look at the URI, and delegate the request to a specific function in a controller. The controller then interacts with a model (if it has to), and procures the data necessary from the Active Record, and initializes variables that will be needed for the next page. Finally, the View with the name matching the URI is rendered, with all the variables in the embedded Ruby filled in based on the initialization in the controller.

By following this framework, Rails standardizes the architecture across projects, although that comes at the cost of flexibility. It also allows you to obey the division of concerns principle, as the Model, the View, and the Controller, all fulfil different functionalities, and you can modularize these functionalities accordingly.


## Why Rails

Many popular and very technically mature websites continue to use Rails. These include: GitHub, Airbnb, Twitter, Hulu, Shopify, and Twitch, among others. Some of the key features that makes Rails the framework of choice for these users are:

### Convention over Configuration

As mentioned before, Rails is an opinionated framework that does things the 'Rails' way. This includes automating trivial tasks by following certain conventions, thus making the developement process much faster. For example, in a Rails project that is connected to a database, each class is mapped to a table, and the table name is just a pluralised version of the class's name - the `User` class becomes the `Users` table, the `Person` class becomes the `People` table, etc. This means that you don't need to spend any time deliberating on the name of the tables, nor do you have to juggle multiple concepts while mapping out the database and application, as Rails will automatically connect the two and spare you from deliberating about the exact name of the table. Much of the boilerplate code, such as configuring routes and running database migrations, are also significantly easier in Rails - unlike other frameworks, where every single route must be mapped to a controller, Rails lets you declare 'Resources', and maps routes for all the CRUD (Create-Read-Update-Destroy) functionalities to a dedicated controller for the resource, even accounting for singular vs plural distinctions.

All of these optimizations help make developing an application between 30-40% faster on Rails. 
  
### Optimised for Programmer Happiness

One of the primary tenets of Ruby was to make it the 'Least Surprising Language', where the language was designed to perform exactly as the developer thought would be the most 'expected' function. Rails founder DHH built on that, and optimized Rails for Programmer Happiness. This meant re-imagining many of coding's most standard conventions, as well as adding significant bloat, all for the purpose of making the code look 'pretty', and inherently 'nice' to write (as subjective as that sounds). 

For example, in order to fetch yesterday's date in Python, one may have to write

```python
import datetime
datetime.datetime.now() - datetime.timedelta(days = 1)
```

The same action in Rails is done using
```ruby
1.day.ago
```

Similarly, Rails also added a new method to access an array element. Instead of following the C convention of using square-brackets to access the second element in the array 'elements' like

```c
elements[1]
```

Rails developers can use ordinal representation to access the same element with the command

```ruby
elements.second
```

And while this may not reduce the amount one has to type, most Rails enthusiasts agree that it makes the code much more readable, intuitive, and inherently happiness-inducing.

### Great Community of Developers

The Ruby language has a very passionate and extensive community of developers, and there is a vast array of libraries (called 'Gems'), that can be used with your Rails projects. These libraries provide a lot of functionalities, and can be used to add flexibility to Rails projects when required.

## Getting Started

The official website for [Rails](https://rubyonrails.org) will have all the information needed to initialize your first Rails application.

To install Rails on your computer, you will first need to install [Ruby](https://www.ruby-lang.org/en/). After this, you simply need to run:

```rb
gem install rails
```
gem is a package manager for Ruby that allows you to install Rails. Once you're done with the setup, you can start developing applications right away. While the [Rails Guides](https://guides.rubyonrails.org/) are the best possible place to learn everything about Rails, [Go Rails](https://gorails.com/) also has great screencast tutorials that really hold your hand through the multitude of features Rails has, and can be used by more visual learners.

</div>

