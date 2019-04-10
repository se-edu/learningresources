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

**Ruby on Rails** is a web application framework written in the Ruby programming language. It is an opinionated framework, that was designed with the intention of making programming web applications easier and quicker by reducing the amount of code you write, instead replacing it with what it considers the "right" way of doing things - what they call the "Rails" way, although it does allow you to follow your own conventions (at the cost of speed and the productivity boost it promises). 

The Rails philosophy includes two major principles, namely:-
1. Don't Repeat Yourself (DRY)
and
2. Convention over Configuration 

### The Rails Way 

One of the core tenets of Rails is 'Convention Over Configuration'. This means that much of the decisions regarding the architecture of the app, as well some other key decisions, are made for you by Rails, and the default application developed by Rails will follow this convention - thus saving you the effort needed to come up with the code necessary to facilitate it. This is perhaps most evident in Rails' decision to follow a Model-View-Controller (MVC) architecture by default.

The structure of apps following an MVC framework is as follows:-

<center>
<img src="MVC.jpeg">

_Figure 1. Model-View-Controller Framework_ <sup>[source](https://medium.com/@matthewmain/rails-request-response-cycle-819e9cd8fa4e)</sup>
</center>

Let us follow along with the diagram, and trace the control flow as a Rails app responds to a request by the client. This app is a standard contacts app, that stores the names of "users" of a software.

#### Router

When Rails receives the URL, it first looks up the URI in the `Routes.rb` file, which defines, by default, all the routes in the application. Each valid URI is mapped to a function located inside a Controller (the 'C' in MVC), that is then invoked to provide a response to the request. 

In order to achieve this in the "Rails" way, it has a convenient abstraction called a "Resource". A resource refers to a RESTful resource that can be acted upon with the HTTP verbs (GET, POST, PUT, and DELETE), and can be Created, have its details Read, Updated, and Destroyed (CRUD). In order to declare a resource, you would mention it inside your `Routes.rb` in the following manner:- 

```ruby
resources :users
```

Now, you can check all the routes that you have in your application by running `bin/rails routes` in your Rails CLI. This should include:

```
                   Prefix Verb   URI Pattern                                                                              Controller#Action
                    users GET    /users(.:format)                                                                         users#index
                          POST   /users(.:format)                                                                         users#create
                 new_user GET    /users/new(.:format)                                                                     users#new
                edit_user GET    /users/:id/edit(.:format)                                                                users#edit
                     user GET    /users/:id(.:format)                                                                     users#show
                          PATCH  /users/:id(.:format)                                                                     users#update
                          PUT    /users/:id(.:format)                                                                     users#update
                          DELETE /users/:id(.:format)                                                                     users#destroy
```

As can be seen, the common routes that you would need for the resources have already been mapped to URI's. Rails will then look for a controller called `userscontroller.rb`, as is convention, and invoke the name of the function mapped to the particular URI and Verb. More information about Rails routes and how they work can be found [here](https://guides.rubyonrails.org/routing.html#resource-routing-the-rails-default).

#### Controller

In the standard Rails convention, once inside the application directory, if you navigate to `app/controllers/`, you should be able to see all the controllers in your project. Rails will look for the controller with the `<name>_controller.rb`, where `<name>` would be replaced by the result under the `Controller` heading in the list of routes above, in this case, "users". It will then look for the function name under the `Action` heading mapped to the URI function inside the Controller, and then call it. This is where the bulk of the business logic of your application would be. So if the person makes a GET request to `/users`, then Rails will open `users_controller.rb`, and call its `index` function.

#### Model

The Model (the M in MVC) is very similar to a Class (à la Object-Oriented Programming), and is a useful abstraction for representing and encapsulating ideas. In Rails, it is also referred to as the "Active Record", as it follows the Active Record pattern. In this pattern, the Models are mapped to tables in the database, and you can query for the fields in the database directly via the model in the native language (i.e. Ruby), instead of making queries the database's language(SQL etc).Models also have generic functions such as find, all, create, save, update, delete included in them by default, so you don't need to implement them or write long queries for each. 

In our diagram, the Controller calls the `all` method in the `User` model, which is, by convention in a file called `User.rb`, which is stored along with all other models at `app/models/`. The model then acts as an intermediary between the application and the database, and returns the results of the corresponding query. 

#### View

After the Controller has fetched all the data necessary and applied whatever transformations are needed, it passes in the requisite fields to its view. The View (the V in MVC) is what the client sees, and is a collection of .erb (Embedded RuBy) files, which are basically just HTML files with lines of Ruby code embedded in it to modify its appearance and behaviour. When the controller is generated using the Rails CLI, a corresponding folder of views is made inside `app/views/`, which is used to store all the .erb files, and that is where Rails will look for files to serve up to the client at the end of the request-response cycle. 

## Why Rails

Many popular and very technically mature websites started off using, and continue to use Rails. These include: GitHub, Airbnb, Twitter, Hulu, Shopify, and Twitch, among others. Some of the key features that makes Rails the framework of choice for these users are:

### Fast iteration speed for product developement 

As mentioned before, Rails is an opinionated framework that does things the "Rails" way. This includes automating trivial tasks by following certain conventions, which can make the development process faster. For example, in a Rails project that is connected to a database, each class is mapped to a table, as described in the Active Record Pattern, and the table name is just a pluralised version of the class's name - the `User` class becomes the `Users` table, the `Person` class becomes the `People` table, etc. This means that you don't need to spend any time deliberating on the name of the tables, nor do you have to juggle multiple concepts while mapping out the database and application, as Rails will automatically connect the two and spare you from deliberating about the exact name of the table. A similar approach is in seen in the usage of resources, since it also automates much of the process.

The automation of these mundane tasks could make development a more enjoyable (more on that in a bit), and faster process. All of these optimizations can make developing an application between 30-40% faster on Rails, while also making it easier to learn, as you do not need to understand HTTP verbs to make a basic Rails application (while mastery of the topic will certainly help). Furthermore, programmers experienced in Rails usually find it easier to start on existing Rails projects, since the conventions followed are usually the same, giving you the same directory structure and route naming conventions everywhere. 
  
### Optimized for Programmer Happiness

One of the primary tenets of Ruby was to make it the "Least Surprising Language", where the language was designed to perform exactly as the developer thought would be the most "expected" function. Rails built on that, and was designed to be optimized for Programmer Happiness. This meant re-imagining many of coding's most standard conventions, as well as adding significant bloat, all for the purpose of making the code look "pretty", and inherently "nice" to write (as subjective as that sounds). 

For example, in order to fetch yesterday's date in Python, one may have to write

```python
import datetime
datetime.datetime.now() - datetime.timedelta(days = 1)
```

The same action in Rails is done using
```ruby
1.day.ago
```

Similarly, Rails also added a new method to access an array element. Instead of following the C convention of using square-brackets to access the second element in the array "elements" like

```c
elements[1]
```

Rails developers can use ordinal representation to access the same element with the command

```ruby
elements.second
```

And while this may not reduce the amount one has to type, most Rails enthusiasts agree that it makes the code much more readable, intuitive, and inherently happiness-inducing.

### Large and Active Community of Developers

The Ruby language has a very passionate and extensive community of developers, and there is a vast array of libraries (called "Gems"), that can be used with your Rails projects. These libraries provide a lot of functionalities, and can be used to add flexibility to Rails projects when required. Furthermore, as a result of its popularity, it is very likely that any problems you encounter with Rails will have a solution that you can find on Google or StackOverflow. If not, the Ruby community is actually very famous for being 'nice', so you're likely to get a response to a new problem very quickly too. 

However, Rails might not be best suited for all use cases, and there are certain areas where other frameworks might be more suitable. For instance:-

### Drawbacks

#### Lack of Flexibility

As demonstrated above, Rails follows very strict conventions, that make life much easier for developers - or so Rails thinks. It might be true that there are some developers who find Rails' convention very stifling, and, as a result, prefer to use frameworks that allow them more flexibility. Rails will still allow you to do as you please, but since the effort of undo-ing the conventions makes the process longer and less enjoyable, it might be better to avoid Rails.

#### Performance Time

While newer releases have combatted this problem to a large degree, many of the old releases of Rails have had a reputation for being very bloated and slow to respond. Applications made using NodeJS, or purely client-side applications with a minimal backend, tend to have a much lower delay between an action and a response. While it should be noted that the vast majority of delays in applications occur due to poor optimization, and not the inherent speed of the framework, Rails still remains more bulky and slower than many of its competitors. 

#### Learning Curve

Rails' tendency to do things "automagically" can also be very confusing to new users, who might be more used to configuring routes and directory structures themselves. Furthermore, it becomes much harder to go from using Rails to a different framework, as the lack of conventions means you will have to learn some new topics to be proficient. You also have to learn the Ruby language, and it might be harder considering that most front-end applications are in JavaScript and its variants, and you will have to juggle multiple languages while you develop your application. 

## Getting Started

To install Rails on your computer, you will first need to install [Ruby](https://www.ruby-lang.org/en/). After this, you simply need to run:

```rb
gem install rails
```
gem is a package manager for Ruby that allows you to install Rails (as well as all the other Gems in Ruby). Once you're done with the setup, you can start developing applications right away. Some resources you might find helpful are:-

1. The [Ruby Guides](https://www.rubyguides.com/) is a must read, as you cannot use Rails without knowing the underlying language.
2. The [Rails Guides](https://guides.rubyonrails.org/) are the definitive guide for Rails and you can be assured that it will always be up to date with the latest releases.
3. [Go Rails](https://gorails.com/) also has great screencast tutorials that really hold your hand through the multitude of features Rails has, and can be used by more visual learners.
4. [Rails Tutorial](https://www.railstutorial.org/book) is a book with a lot of advanced topics, so if you really want to study Rails in depth, consider this.
</div>

