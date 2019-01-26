<frontmatter>
  title: Introduction to Databases & Database Management Systems (DBMS)
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Introduction to Databases & Database Management Systems (DBMS)

Authors: [Syed Abdullah](https://github.com/Skaty)

## Why learn databases and DBMSes?

The programs that we create would handle data in some way or another. Be it a simple calculator application that tabulates user calculations (and maybe store them in a log of recent calculation) or a cluster of servers that handle indexing of a large number of documents. Data is what is worked on by any program, the inputs and outputs of a program or even a simple function are data.

As our programs get larger and the data that is worked on becomes much more complex, there needs to be a way for us to systematically store and access data that is being worked on. It could be a crude structure, designed specifically for a particular use case or something that is more standardised, for instance, an application that handles data.

This notion of structuring data and providing an easy to use abstraction for accessing, storing and performing mundane operations on data (for instance, sorting them) is what this guide will cover.

## Introduction

![DB-DBMS Relationship](images/db-dbms-relation.png)

A **database** is a collection of data. While any data in a medium can theoretically be a database (for instance, scribbling on a piece of paper), databases in the context of this chapter typically involve data that are organised in some manner.

Programs generally do not access the raw data directly. Instead, a  **database management system (DBMS)** is used, which will handle the storage, retrieval and updating of the data.

Sometimes people use the term database to refer to a DBMS. However, to avoid any confusion, this guide will use these terms as defined above.

There are various concepts in the field of databases and DBMS and this guide will cover the basic concepts that are useful for someone who is starting out.

## Database models

For databases to make any sense, there has to be a certain logical structure in the database (for instance, how data is stored). This logical structure is known as a **database model**.

There are many types of database models. Some examples are:

- [**Relational model**](https://en.wikipedia.org/wiki/Relational_model) - the most widely used database model, usually modeled using a table format. The data structure is defined by a set of relations and domains (types) that dictates the constraints. Constraints are also established for the type of operations that can be done.
- [**Document-oriented**](https://en.wikipedia.org/wiki/Document-oriented_database) - data is stored in documents, that encapsulates the data. This data is normally stored in a semi-structured manner.
- [**Key-value**](https://en.wikipedia.org/wiki/Key-value_database) - utilises a dictionary-like structure
- [**Graph database**](https://en.wikipedia.org/wiki/Graph_database) - utilises graph structures to store data
- [**Flat**](https://en.wikipedia.org/wiki/Flat_file_database) - data stored as files, without any structure
- [**Multi-model**](https://en.wikipedia.org/wiki/Multi-model_database) - DBMS that supports multiple database models

[Click here](https://en.wikipedia.org/wiki/Database_model) to learn more about the different database models.

### Why are there different models?

There are advantages and disadvantages to utilising one of the many database models available. These different models seek to solve certain issues when programs deal with data.

For instance, the relational model is the most structured out of the three that were shown. The presence of structure allows the database to enhance and improve certain common operations, for instance, searching for a specific data for an entry would be faster.

However, having such a rigid structure would mean that there's a limitation on how and what kind of data can be stored in the database.

For the purposes of introduction, we would mainly cover on aspects that are used in the relational and/or document-oriented database models.

### Examples of DBMS implementations

- [MongoDB](https://www.mongodb.com/) (document-oriented)
- [MySQL](https://www.mysql.com/) (relational)
- [PostgreSQL](https://www.postgresql.org/) (relational)
- [ArrangoDB](https://www.arangodb.com/) (document, graph, key-value - multi-model)
- [Redis](https://redis.io/) (key-value database)

## Relations

The most popular database model, relational model, makes use of relations. This model assumes that the data to be stored follow a certain 'pattern'. For instance, a database that stores products sold by a shop would contain data such as: name, description, price and current stock levels. A visualisation of the database and data can be seen below:

| Name  | Description                  | Price | Current Stock |
|:-----:|:----------------------------:|:-----:|:-------------:|
| Fruit | A fruit.                     | 1.00  |      100      |
| Bread | Sliced for your convenience. | 1.40  |       50      |
| Water | Essential for life.          | 0.50  |     1000      |

From this visualisation, we can define the different parts of the relation:

- The whole table is known as a **relation**.
- Each data row (i.e. excluding the header row) is known as a **tuple**, for instance: (Fruit, A fruit., 1.00, 100).
- Each column is an **attribute** and each attribute has a **domain** or **data type**. The *Current Stock* column of the table is an attribute with a data type of integer (as product stock is logically represented using an integer).
- Each element in a tuple is called an **attribute value**

[Click here](https://en.wikipedia.org/wiki/Relation_(database)) for a more in-depth and formal definition of relations.

## Transactions

Databases are useless if the data cannot be used in a meaningful manner. However, uncontrolled access to the database would not be ideal, as it might cause problems, especially when other actions may depend on the previous action's result.

Take for instance, a program that transfers money from one bank account to another. The actions that the program would need to do involves:

1. Checking if the sender has enough money
2. Deducting the amount of money to be sent
3. Adding that amount to the receiver's account

A problem arises if another action takes place in between any of the steps (e.g. another transfer from the same sender), or if any of the steps fails (e.g. due to a program crash). As a result, it may cause the data to be manipulated in an undesired manner (e.g. money not credited to receiver).

Thus, **transactions** allow us to guard against these problems. A **transaction** symbolises a logical unit of work, which consists of multiple database actions, performed on a set of databases. [[Source]](https://en.wikipedia.org/wiki/Database_transaction) Properties of a database transaction ensures that these actions are done in a predictable (i.e. in the particular order) and reliable (i.e. all actions must be performed correctly) manner.

Thus, the transaction that would be implemented in the program could be something like this:

1. Perform funds transfer from *sender* to *receiver*
    - Checking if the sender has enough money
    - Deducting the amount of money to be sent
    - Adding that amount to the receiver's account

As demonstrated above, the actions that needs to be done in order to transfer funds is wrapped as one large **transaction**. Hence, the *funds transfer* can be seen as the unit of work to be done on the database. The actions that make up the **transaction** are executed as though **transaction** is a single action.

### ACID in transactions

For a **transaction** to be considered as an implementation of the **transaction** concept, it has to satisfy the ACID principle.

This principle states that a **transaction** must contain these characteristics:
- Atomicity - transactions only succeed if all parts of the transaction succeeds. That is to say, if any action fails, the transaction fails and the state of the database should be left unchanged (i.e. as if the transaction did not happen)
- Consistency - transactions must ensure that the database remains in a valid state after the transaction (for instance, all relations hold true)
- Isolation - if multiple transactions runs at the same time, the result should be the same as though these transactions are run sequentially
- Durability - transactions and its results should remain persistent (i.e. power loss or reboot should not affect results)

## Distributed Databases

The above section demonstrates how data can be related to each other. However, this demonstration assumes one thing: there is only one record of the data that is stored. What if there is a need to scale the database in such a way that the data is distributed across several servers?

Relational model DBMSes usually do not scale as well, as the **ACID principle**, more specifically, durability, forces the database to propagate any changes to the data across all servers. One famous theorem, the **CAP theorem**, states the a distributed computer system can only fulfil two out of three guarantees.

|      Guarantee      |                          Description                         |
|:-------------------:|:------------------------------------------------------------:|
| Consistency         |               Read should receive latest write               |
| Availability        |         Every request receives a response (non-error)        |
| Partition tolerance | System works even though there are some communication errors |

A relational model DBMS trades off availability for consistency. As the changes are propagated across the network, subsequent requests might be dropped by the DBMS as the current state of the database violates ACID.

However, in other DBMSes, like MongoDB, consistency is the trade off. This allows the database system to scale up to multiple nodes, as all requests are served, but the requests may result in incorrect or out of date data.

As such, these DBMSes follow the **BASE philosophy**:

- Basically Available - data is guaranteed available, but data may not be retrieved correctly (i.e. unable to retrieve or incorrect/out of date data)
- Soft state - state of system changes even though there might not be any user input, as it needs to ensure 'eventual consistency'
- Eventual consistency - the consistency of the system eventually occurs, but changes to data are still accepted in the meantime

## Database Abstractions and Languages

### Relational Algebra

A formal method of modelling the relations that have been demonstrated in this chapter is through the use of **relational algebra**. This is a formal method for modelling the data and actions performed on a relational database.

#### Further exploration

- [Relational algebra](https://www.tutorialspoint.com/dbms/relational_algebra.htm)

### Query Language and Abstractions

We have seen how databases are structured and how the underlying DBMS ensures that a certain set of characteristics, with regards to the system, hold true.

Now, the data consumer (for instance, an application or an actual human) would preferably want to access the data in a manner that is not DBMS specific. The DBMS implementation should have very little effect on the actual method of accessing the data. If there's a need to switch over to a different DBMS that has the same set of features as the previous DBMS, the application should preferably not have to change its method of accessing the data.

**Query languages** solves that issue, as some of them are designed to be platform-independent. As such, the query language can be seen as an abstraction of the possible actions that can be performed on a specific set of DBMSes. However, be forewarned that query languages are not totally platform-independent, as certain DBMSes may implement features that are unique to the certain DBMS.

Take for instance SQL, which is one of the most popular query languages for relational DBMSes. While most features in the language are supported by relational DBMSes that uses SQL, certain features, for instance `SAMPLE` (which allows the consumer to pick a random set of data) are not available on all of the DBMses that supports SQL.

Another level of abstraction is the **database abstraction layer**. This is usually an API level solution, as the programmer does not even need to know about a specific query language. Some abstractions are DBMS agnostic and as such, can be used to access data from any kind of DBMS, regardless of its features.

#### Further exploration

##### Query languages

- [SQL](https://www.w3schools.com/sql/)
- [XPath](https://www.w3schools.com/xml/xpath_intro.asp)

##### Database abstractions

- [PHP Data Objects](https://secure.php.net/manual/en/book.pdo.php)
- [Object-relational mapping](https://en.wikipedia.org/wiki/Object-relational_mapping)
</div>
