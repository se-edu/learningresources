# Scalable Development - An Introduction

Authors: Loh Jia Shun Kenneth

# Scalable Development
When Pokémon Go first launched in July 6, the heavy traffic from users caused its servers to crash. However, as of 2017, Google handles at least 3.5 billion searches daily, Whatsapp handles at least 10 billion messages a day, and Facebook hosts 1.2 billion daily active users. What makes these companies different?

The key difference is their server infrastructure. A scalable server allows a company to provide reliable service even as the user base increases. Even if your service might not have to cater to millions of users at the moment, designing scalable software now will reduce the need to restructure the entire backend when the need arises.

# Scalability

So what is scalability? Scalability is the capacity to which a system can cope with an increased workload. Workload can refer to number of users on the server, number of files uploaded, number of concurrent requests, or any quantity that the system needs to scale for.

# Ways to scale

Let's say you have a server running on a single host, but due to the growing number of users, it has been slowing down. What are the different ways you can speed it up?

## Get A Better Host

This is arguably the easiest way to speed up your server, as it is a general solution to most problems. Simply get faster CPUs, larger RAM, etc. This is called vertical scaling. If you get multi-core processors, use threading in your server to allow parallelization of certain processes to take advantage of the cores.

However, due to Amdahl's law, each additional processor will give a less-than-proportionate speed up. Coupled with the exponential increase in price as the computing power of the CPU increases, this is typically not scalable in the long run.

## Faster Algorithms

If you have computationally-intensive functions, making them faster can help save a lot of time. For instance, if you want to look up files stored with a certain tag, a hashtable can be used to store files under commonly-searched tags, as opposed to a linear search. The difference between O(1) and O(n) increases as the number of files on a server.

In certain scenarios, the runtime complexity can be reduced as a trade-off between time and accuracy. For example, if you want to figure out the type of content a user wants to see on a social network, rather than analyze the user's past history of posts liked, you can just sample the 100 most recent post liked by the user and analyze them.

## Avoid Bottlenecks

There are a few common bottlenecks that can be avoided such as database querying, reading or writing to files and slow communication across network. Different bottlenecks can be solved with their respective solutions. Use non-relational databases where feasible, as they are scalable. Cache recent or frequently-accessed data to reduce database querying and file I/O. Avoid large data movement across the network by keeping the data close to the computation.

There are many program profilers available that you can use to find out the bottleneck in your server. By finding out where the server spends most computational time in, you can potentially save huge amounts of time.

Performance bottlenecks ([resource](https://www.apicasystem.com/blog/5-common-performance-bottlenecks/))

Non-Relational Database ([resource](http://www.jamesserra.com/archive/2015/08/relational-databases-vs-non-relational-databases/)) ([resource](https://www.pluralsight.com/blog/software-development/relational-non-relational-databases))

## Get More Machines

Buy more machines to host your server. This is called horizontal scaling. This method can help improve scalability by a huge factor, as each additional unit of processing power and RAM costs the same, no matter how many machines you buy. As this is a promising way to increase scalability, It is no surprise why large services such as Google and Facebook use thousands of computer servers just to provide service to all their users.

However, to fully reap the benefits of multiple machines, you will need to learn various techniques to modify your server infrastructure. In most cases you cannot simply run multiple instances of the server and expect it to work.

Here are some tips to get started with horizontal scaling:
- Learn how to utilize MapReduce on [Hadoop](https://www.tutorialspoint.com/hadoop/index.htm). The Hadoop ecosystem is so versatile and widely-used that there are companies offering Hadoop as a service, so you do not have to set up the server yourself
- Host your server on [Google App Engine](https://cloud.google.com/appengine/docs). Despite being more expensive than Amazon Web Services and DigitalOcean, it offers very scalable infrastructure for your server.
- Run a server with multiple nodes with MPI. [Open MPI](https://www.open-mpi.org/) is an open source message-passing library that can be used to send data between the nodes in your server. You might have to study [cloud computing infrastructure](http://whatiscloud.com/basic_concepts_and_terminology/it_resource) in order to make use of this.
- Study various scalable architecture for servers ([resource](http://srinathsview.blogspot.sg/2011/10/list-of-known-scalable-architecture.html)).

# But wait!

Before you start tweaking your server to make it scalable, be sure to avoid the following pitfalls:
- Do not optimize prematurely or unnecessarily: your "smart" tweak might save your server 3 seconds a day in return for making your life a living hell when you try to debug it 3 months later.
- Do not start without a performance check: if processing an image seems to take a long time, you might spend days optimizing the processing function, only to find 99% of the time is spent sending it from one server to another.
- Do not forget the goal: deliver the product first. There is no point hosting a server for millions of people when you cannot even hit a hundred users. Get your users first, then start worrying about scalability.

# Conclusion

There is a huge gap between theory and practical. What looks well on paper might need tweaking with arbitrary constants and "hacks". As such, more research should be done before you implement any of the solutions.

However, scalability will still be a crucial part of servers aiming to provide services to an increasingly-growing user base. Learning good scalability practices will help prevent developing a server that will never be able to scale. Take the time to explore the depths of scalability, and you will be able help your server scale to meet its demand.