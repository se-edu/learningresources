# Introduction to Databases & Database Management Systems (DBMS)

Authors: [Syed Abdullah](https://github.com/Skaty)

## Introduction

![DB-DBMS Relationship](images/db-dbms-relation.png)

A **database** is a collection of data. While any data in a medium can theoretically be a database (for instance, scribbling on a piece of paper), databases in the context of this guide typically involve data that are organised in some manner.

Programs generally do not access the raw data directly. Instead, a  **database management system (DBMS)** is used, which will handle the storage, retrieval and updating of the data.

Sometimes people use the term database to refer to a DBMS. However, to avoid any confusion, this guide will use these terms as defined above.

There are various concepts that are used in the context of databases and DBMS. This guide will cover the basic concepts that are useful for a beginner to start out with.

## Database models

For databases to make any sense, there has to be a certain logical structure in the database (for instance, how data is stored). This logical structure is known as a **database model**.

There are many types of database models. Some examples are:

- **Relational model** - the most widely used database model, usually modeled using a table format. The data structure is defined by a set of relations and domains (types) that dictates the constraints. Constraints are also established for the type of operations that can be done.
- **Document-oriented** - data is stored in documents, that encapsulates the data. This data is normally stored in a semi-structured manner.
- **Flat** - data stored as files, without any structure

[Click here](https://en.wikipedia.org/wiki/Database_model) to learn more about the different database models. However, for the purposes of introduction, we would mainly cover on aspects that are used in the relational and/or document-oriented database models.

## Relation

The [proper, formal definition by E. F. Codd](https://en.wikipedia.org/wiki/Relation_(database)) would not be covered here, as it requires the understanding of first-order logic. As such, this section will cover this concept in a simplified manner.

The most popular database model, relational model, makes use of relations. This model assumes that the data to be stored follow a certain 'pattern'. For instance, a database that stores products sold by a shop would contain data such as: name, description, price and current stock levels. A visualisation of the database and data can be seen below:

| Name  | Description                  | Price | Current Stock |
|:-----:|:----------------------------:|:-----:|:-------------:|
| Fruit | A fruit.                     | 1.00  |      100      |
| Bread | Sliced for your convenience. | 1.40  |       50      |
| Water | Essential for life.          | 0.50  |     1000      |

Assume that the above table models a database utilising a relational model.

- The whole table is known as a **relation**.
- Each data row (i.e. excluding the header row) is known as a **tuple**, for instance: (Fruit, A fruit., 1.00, 100).
- Each column is an **attribute** and each attribute has a **domain** or **data type**. The *Current Stock* column of the table is an attribute with a data type of integer (as product stock is logically represented using an integer).
- Each element in a tuple is called an **attribute value**

## Transactions

Databases are useless if the data cannot be used in a meaningful manner. However, uncontrolled access to the database would not be ideal, as it might cause problems, especially when other actions may depend on the previous action's result.

Take for instance, a program that transfers money from one bank account to another. The actions that the program would need to do involves:

1. Checking if the sender has enough money
2. Deducting the amount of money to be sent
3. Adding that amount to the receiver's account

A problem arises if another action takes place in between any of the steps (e.g. another transfer from the same sender), or if any of the steps fails (e.g. due to a program crash). As a result, it may cause the data to be manipulated in an undesired manner (e.g. money not credited to receiver).

Thus, **transactions** allow us to guard against these problems. A **transaction** symbolises a unit of work performed on a set of databases. [[Source]](https://en.wikipedia.org/wiki/Database_transaction) This unit of work can consist of multiple actions to be performed. Properties of a database transaction ensures that these actions are done in a predictable and reliable manner.

### ACID in transactions

For a *transaction* to be considered as an implementation of the **transaction** concept, it has to satisfy the ACID principle.

This principle states that a **transaction** must contain these characteristics:
- Atomicity - transactions only succeed if all parts of the transaction succeeds. That is to say, if any action fails, the transaction fails and the state of the database should be left unchanged (i.e. as if the transaction did not happen)
- Consistency - transactions must ensure that the database remains in a valid state after the transaction (for instance, all relations hold true)
- Isolation - if multiple transactions runs at the same time, the result should be the same as though these transactions are run sequentially
- Durability - transactions and its results should remain persistent (i.e. power loss or reboot should not affect results)

## Topics for Further Exploration

As this chapter serves to introduce readers to the basics of databases, there are a lot of concepts that are not covered in this chapter.

Now that you know the purpose and characteristics of databases and DBMS, why not go deeper? Here are some topics relating to databases for further exploration by the reader.

- Relational algebra \& calculus
- Domain-specific languages
  - SQL
- Specific DBMS implementations
  - MongoDB
  - MySQL
  - PostgreSQL
- Abstraction of databases

## Resources

- [TutorialsPoint - Tutorial on DBMS](https://www.tutorialspoint.com/dbms/)
- [Getting started on MySQL](https://dev.mysql.com/doc/refman/en/tutorial.html)

// references and learning resources