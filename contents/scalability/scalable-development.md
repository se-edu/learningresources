<frontmatter>
  title: Scalable Development - An Introduction
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Scalable Development - An Introduction

Authors: Loh Jia Shun Kenneth, Vivek Lakshmanan

<box id="article-toc">

* [Scalable Development‎](#scalable-development)
* [Scalability‎](#scalability)
* [Ways to scale‎](#ways-to-scale)
  * [Horizontal vs Vertical Scaling‎](#horizontal-vs-vertical-scaling)
  * [Faster Algorithms‎](#faster-algorithms)
  * [Avoid Bottlenecks‎](#avoid-bottlenecks)
* [But wait!‎](#but-wait)
* [Conclusion‎](#conclusion)
</box>

## Scalable Development
When Pokémon Go first launched in 2016, the heavy traffic from users caused its servers to crash as the server was built to handle an expected maximum of five times the average network traffic when in reality, it was fifty times that. However, as of 2017, Google handles at least 3.5 billion searches daily, Whatsapp handles at least 10 billion messages a day, and Facebook hosts 1.2 billion daily active users. What makes these companies different?

The key difference is their server infrastructure. A scalable server allows a company to provide reliable service even as the user base increases. Even if your service might not have to cater to millions of users at the moment, designing scalable software now will reduce the need to restructure the entire backend when the need arises.

## Scalability

So what is scalability? Scalability is the capacity to which a system can cope with an increased workload. Workload can refer to number of users on the server, number of files uploaded, number of concurrent requests, or any quantity that the system needs to scale for.

## Ways to scale

Let's say you have a server running on a single host, but due to the growing number of users, it has been slowing down. What are the different ways you can speed it up?

### Horizontal vs Vertical Scaling

#### Horizontal Scaling

Horizontal scaling is simply buying more machines to host your server. This method can help improve scalability by a huge factor, as each additional unit of processing power and RAM costs the same, no matter how many machines you buy. As this is a promising way to increase scalability, it is no surprise why large services such as Google and Facebook use thousands of computer servers just to provide services to all their users.

However, to fully reap the benefits of multiple machines, you will need to learn various techniques to modify your server infrastructure. In most cases you cannot simply run multiple instances of the server and expect it to work.

Here are some tips to get started with horizontal scaling:
- Understand [Parallel Computing](http://web.eecs.umich.edu/~qstout/parallel.html) and how to use it to speed up computation. 
- Learn how to utilize MapReduce on [Hadoop](https://www.tutorialspoint.com/hadoop/index.htm). The Hadoop ecosystem is so versatile and widely-used that there are companies offering Hadoop as a service, so you do not have to set up the server yourself.
- Host your server on [Google App Engine](https://cloud.google.com/appengine/docs). Despite being more expensive than Amazon Web Services and DigitalOcean, it offers very scalable infrastructure for your server.
- Run a server with multiple nodes with MPI. [Open MPI](https://www.open-mpi.org) is an open source message-passing library that can be used to send data between the nodes in your server. You might have to study [cloud computing infrastructure](http://whatiscloud.com/basic_concepts_and_terminology/it_resource) in order to make use of this.
- Study various scalable architecture for servers ([resource](http://srinathsview.blogspot.sg/2011/10/list-of-known-scalable-architecture.html)).

#### Vertical Scaling

This is arguably the easiest way to speed up your server, as it is a general solution to most problems. Simply get faster CPUs, larger RAM, etc. This is called vertical scaling. If you get multi-core processors, use threading in your server to allow parallelization of certain processes to take advantage of the cores.

However, due to Amdahl's law, each additional processor will give a less-than-proportionate speed up. Coupled with the exponential increase in price as the computing power of the CPU increases, this is typically not scalable in the long run.

#### Should you choose one or the other?

With vertical scaling, you get to take advantage of processing power and concurrency. But as mentioned above, this isn't sustainable over the long run because it: 
1. Results in a single point of failure. 
1. Prevents changes to be made to the code without bringing the entire system down.
1. Hinders the ability to distribute your application geographically. 

You can solve these issues with horizontal scaling. But with horizontal scaling alone, what each machine can do will be much lesser than with vertical scaling. As a result, it's fairly hard to choose between the two and in the end, it boils down to using both horizontal and vertical scaling in order to achieve optimal performance. 

### Faster Algorithms

If you have computationally-intensive functions, making them faster can help save a lot of time. For instance, if you want to look up files stored with a certain tag, a hashtable can be used to store files under commonly-searched tags, as opposed to a linear search. The difference between O(1) and O(n) increases as the number of files on a server increases.

In certain scenarios, the runtime complexity can be reduced as a trade-off between time and accuracy. For example, if you want to figure out the type of content a user wants to see on a social network, rather than analyze the user's past history of posts liked, you can just sample the 100 most recent posts liked by the user and analyze them.

### Avoid Bottlenecks

There are a few common bottlenecks that can be avoided such as database querying, reading or writing to files and slow communication across network. Different bottlenecks can be solved with their respective solutions. Here are some ways to avoid bottlenecks.

#### Caching
With an increasing user base, your server has to deal with a larger number of requests along with the bottlenecks mentioned previously such as network congestion and database querying. As a result, the response becomes slower. This is where [caching](http://www.lecloud.net/post/9246290032/scalability-for-dummies-part-3-cache) comes in. A cache is a key-value store that resides between the application and the database which can either be in the browser or part of your server infrastructure itself. By retrieving data from the cache instead of the database, the data is retrieved locally (either from the browser or web server) instead of remotely from a database. Simply put, data is retrieved from a closer location and as a result, the response time reduces greatly.

The next thing to consider is [what to cache](https://www.infoq.com/articles/ebay-scalability-best-practices). The rule of thumb is to cache data that is:
1. Frequently accessed.
1. Read more often than it is updated. 

With this, cache hits would be more often than cache misses and as a result, the time saved from faster accesses and reads outweighs the extra time taken to populate the cache.  

#### Sharding 
As your traffic increases your data increases as well and as a result, your database gets overloaded. One way to mitigate this is to scale your database by sharding. [Sharding](https://stackoverflow.com/questions/992988/what-is-sharding-and-why-is-it-important) is a method of splitting and storing a single logical dataset in multiple databases. More specifically, it is the storing of data horizontally - storing rows of a same table in multiple database nodes instead of storing them in the usual vertical way - storing different tables & columns in a separate database.

The [benefits of Sharding](https://medium.com/@jeeyoungk/how-sharding-works-b4dec46b3f6) include:
* Sharding allows you to store more data - Sharding is essential when your dataset becomes too large to store in a single database. It reduces the number of rows in each table and as such improves search performance since the search is done on a smaller table. 

* Sharding allows for quicker query response time - Querying the databases containing only the relevant partitions becomes possible as well. For instance, if a database contains a column for age, you can partition the rows according to an age group and store them in different databases. Whenever there is a need to access the data of a particular age group, instead of querying the whole database, you just need to query the partition that contains that age group. This allows your database to scale along with your data and traffic growth.

#### Go Asynchronous 
Unlike synchronous operations that run sequentially and wait for previous operations to complete, [asynchronous operations](https://www.youtube.com/watch?time_continue=2&v=8aGhZQkoFbQ) do not block further execution and as a result, other operations do not have to wait.

In situations in which responding to request is crucial, asynchronous operations can reduce the latency experienced by the requester and thereby avoid that bottleneck by prioritising the quickness of the response to the user over the speed of other processes such as execution latency (how quickly the request is processed). For instance, rather than waiting for some processes such as downloading the requested file to finish, it's better to asynchronously update the user interface and then finish these processes. In this way, the user experiences lesser latency since the user interface is updated instantly instead of appearing to have crashed due to waiting for the downloaded file.

You can use the many program profilers available to find such [common bottlenecks](https://www.apicasystem.com/blog/5-common-performance-bottlenecks) in your server, thereby potentially saving huge amounts of time.

## But wait!

Before you start tweaking your server to make it scalable, be sure to avoid the following pitfalls:
- Premature or unnecessary optimization: a tiny performance improvement that can make your code hard to understand later on is not worth it.
- Optimizing without profiling performance: optimizing the function that takes up 1% of the delay will not improve a lot.
- Forget the product: if there is no product, then scaling your server to handle a million users is a moot point.

Use the following tips to avoid those pitfalls:
- Structure the code so that it will be easy to scale up. For instance if you know a portion of your code can be implemented with MapReduce, code it so that it follows the MapReduce structure.
- Implement code that is easy to understand. If your "smart" tweak saves you half a millisecond makes your code hard to understand, then it probably is a premature optimization.
- Always profile your program's performance first. Be sure to find out what is causing a huge slowdown, then optimize that portion.
- Test if your improvement actually speeds up the runtime in practice.
- If trying to optimize a certain portion to run slightly faster will take you a few days, avoid that. Find a balance between delivering the product or features, and scalability.

## Conclusion

There is a huge gap between theory and practice. What looks well on paper might need tweaking with arbitrary constants and "hacks". As such, more research should be done before you implement any of the solutions.

However, scalability will still be a crucial part of servers aiming to provide services to an increasingly-growing user base. Learning good scalability practices will help prevent developing a server that will never be able to scale. Take the time to explore the depths of scalability, and you will be able help your server scale to meet its demand.
</div>
