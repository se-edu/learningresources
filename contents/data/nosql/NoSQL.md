<frontmatter>
  title: Introduction to NoSQL
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Introduction to NoSQL

**Author(s): [Ang Ze Yu](https://github.com/ang-zeyu)**

**Reviewer(s):** [Neil Brian](https://github.com/nbriannl)

---

## What is NoSQL?

**Non-Structured Query Language** (**NoSQL**) is a wide set of implementations of query technologies used to retrieve and store data in a non-tabular format.

Some common implementations of such databases are key-value stores, document stores (which are an extension of key-value stores),
and though less common - graph databases (which can also be considered an extension of document stores).

---

## How does NoSQL work?

One such commonly used database is MongoDB, which is a document based database.

### Basic structure of document-based databases

![](documentDatabaseStructure.png)

In contrast to tables and table entries in SQL databases, we typically have collections and documents.
A database comprises of multiple collections, which in turn consists of multiple documents.

In a simplified e-commerce website for example, you may have the following collections:
- customers - storing the account details of customers, their purchase histories, etc.
- items - a collection of all items available for purchase (which are documents) 
- admin - a collection storing admin account details
- **...**

The following might be how an item document available for purchase under the *items* collection
is represented in a JSON format.


```js
{
  type: "book",
  price: 20,
  popularity: 9.7,
  ...
}
```

<br>
<box type="info" light>
Note that while many NoSQL databases provide a JSON interface to interact with the data,
the underlying storage implementation may be different for performance reasons.<br>
For example, MongoDB stores documents in BSON (json encoded in binary). 
</box>

<br>

### Basic CRUD operation examples

Let's skip the trivial details of database setup which would differ across different database technologies,
and look at how you would interact with NoSQL databases.

Unlike SQL, where database interaction is achieved through the different dialects of the query language,
most interaction with NoSQL databases is achieved in a simple and intuitive _object oriented manner_,
and JSON-like queries.

Let's get back to the above example. Say you already have the _customers_ collection set up and ready to go.
A potential new user sends a POST request to register a new account, and your server would then simply call
the following mongoDB code to store the details after some processing.


```js
// Example insertion of a customer document into the _customers_ collection
db.customers.insertOne({
  username: "panda",
  password: hashedPassword,
  email: "panda@pandas.com"
})
```

That was a rather simple use case, but you get the idea. Let's get into something more powerful.

<div id="schemaless">

Suppose our _items_ collection is structured like so:
</div>

```js
[
  {
    type: "book",
    title: "about pandas",
    price: 20,
    popularity: 9.7,
    author: "panda1",
    ...
  },
  {
    title: "more pandas",
    author: "panda2",
  },
  ...
]
```

And a customer filters through your catalogue with a price of less than 30, and a popularity with more than 8.
Furthermore, he / she sorts the items lexicographically by the price, then by descending order of the title.

To retrieve that data, your backend would make a query like so:
```js
db.items.find({
  price: { $lt: 30 },
  popularity: { $gt: 8 }
}).sort({
  // Here 1 means ascending order, and -1 means descending
  price: 1,
  title: -1
})
```

Here, the code simply provides a query JSON object matching the structure of an item document.

Specifically, items with a price of less than 30 are filtered with the `$lt: 30` key-value pair,
and items with a popularity of more than 8 are filtered with the `$gt: 8`.<br>
Subsequently, a "cursor" is returned, which undergoes a `sort` operation by the respective fields of `price`
and `title`.

CRUD operations with NoSQL databases are just as powerful as traditional SQL databases, and in some
cases even more succinct.

<br>

### Schemas in NoSQL

One key characteristic of most NoSQL databases is that they are **schema-less**.

This means that each individual document has no restriction on what keys it must have,
the number of keys, the type of values, etc.
(Note the missing fields for the second item in the earlier code example [above](#schemaless), which are intentionally omitted.)

Documents can even contain other documents (json objects), arrays - anything the database used
can serialize and deserialize.

#### Validation

At the same time, NoSQL databases usually also provide some degree of schema validation.

For example, in the _customers_ collection, where the fields of a customer are unlikely to
change, it can be helpful to enforce a strict schema on documents to prevent erroneous database
operations.

```js
...
{
  $jsonSchema: {
    bsonType: "object",
	require: [ "username", "password", "email" ]
	properties: {
	  ...
	}
  }
}
...
```

<br>

### Relation storage in NoSQL

The world is full of relations. For example, a patient _is related to_ his / her disease record,
just as a customer _is related to_ their shopping cart.

Sometimes, the objects on both sides of the relation can contain substantial amounts of
information, and may be impossible to store as a singular field in one or the other document.

Hence, simple relations such as one-to-one relations, one-to-many relations are often expressed in NoSQL
databases directly in the form of embedded documents.

For example, for a customer and his / her shopping cart, we may have the following:
```js
{
  username: "panda",
  cart: {
    totalPrice: 100,
    cartItems: [ ... ],
    discountCode: "panda"
  },
  email: "panda@pandas.com",
  ...
}
```

Such simple relations are extremely suited to and expressable with the schema-less characteristic
of NoSQL databases.

In the case of more complicated many-to-many relationships, references are commonly stored using references,
to avoid duplication of data.

For example, items in an e-commerce website are related to the many customers through their carts.
In these carts, it is much more space efficient to store references (to the items), than the item documents themselves.

Let's introduce a uniquely generated `_id` field for each item document in the items collection:
```js
{
  type: "book",
  price: 20,
  popularity: 9.7,
  _id: 9d1793bd491349n913847n93d
}
```

In the user's cart, we would simply store these item `_ids`, which are used to lookup the item documents in the items collection later:
```js
...
cart: {
  totalPrice: 100,
  cartItems: [
    "9d1793bd491349n913847n93d",
    "9d1793bd491349n913847njh8",
  ],
  discountCode: "panda"
}
...
```



<box type="info" icon-size="2x">
For this reason, many NoSQL database solutions (e.g. MongoDB) implement a unique <code>id</code> field
for each document by default.
</box>

<br>

### Horizontal scaling :fas-server:

Another key characteristic of NoSQL databases is ability to scale horizontally (distributing workload across
multiple servers), without discarding much of its key features.

This is largely due to the schema-less architecture of such databases, allowing data to split across
multiple servers easily and efficiently.

For example, take the following collection of items with a `title`:

```js
[
  {
    title: "let's assume item titles are lexicographically distributed uniformly over the long run"
    type: "book",
    price: 20,
    popularity: 9.7,
  },
  // item 2
  // item 3
  // ....
  // item N
]
```

Assuming we don't have relations from items to themselves inside these documents,
we can split the collection like so:

<img src="horizontalScalability.png" width="500" />

As a result, the database access workload can be distributed evenly and efficiently across
multiple servers easily.

---

## Why NoSQL?

<br>

### 1. Highly suited for agile development :dash:
Although less mature than SQL databases, NoSQL databases were designed to solve many of the emerging challenges in databases today.

One of the most consequential impacts NoSQL has had was enabling faster agile development.
Given the highly flexible relational structure of NoSQL databases, and the schemaless format of documents 
in NoSQL, this means that developers can adapt the database quicker to changing customer and business requirements.


### 2. Horizontal scalability
As explained above, NoSQL databases are easily horizontally scalable.

As businesses grow and see increasing amounts of web traffic, it is crucial that its databases can scale to meet
consumer and business demands.
Vertical scaling (increasing the processing power of the machine) can only go so far until the single machine hits its limit.

### 3. Widespread adoption

![](databaseSurvey.png)

While certainly trending behind SQL databases, NoSQL databases have been booming over the past couple of years,
due to the increasing applicability of its benefits to requirements today. (Amazon uses a proprietary NoSQL database!)

This bodes well for the maturity and development of this evolving technology, and your potential use cases for it.

---

## Caveats of NoSQL

<br>

#### 1. Lack of standardisation
From both a user and implementation standpoint, NoSQL databases vary from one solution to another greatly, which can incur
extra development costs in projects when there is a need to migrate to another solution, or when new developers are introduced
to the project.

This is in stark contrast to SQL, which's syntax is mostly standardised across its different flavours.

#### 2. Not suited for complex relational queries
While NoSQL databases certainly allow for more flexibility in structuring out relations, most complex queries (e.g. joins for many-many relations) _usually_ involve structured data that can be easily represented in tabular formats. 

In such instances, queries are often more performant in SQL equivalents.


---

## How to get started with NoSQL?

There are many NoSQL variants out there as mentioned earlier.<br>
For starters, it may be wise to go with the most common solution, _mongoDB_.

You should first follow [the mongoDB documentation](https://docs.mongodb.com/manual/installation/#mongodb-community-edition-installation-tutorials)
and learn to set up a local instance of mongoDB, which will be necessary for interacting with mongoDB through any application drivers.

Thereafter, you should use a **local** _mongo shell_ to get familiar with mongoDB syntax.
To do so, you can follow the instructions [here](https://docs.mongodb.com/manual/mongo/) to connect to your 
mongoDB instance from the shell as you had configured.

<box type="info" icon-size="2x">

There are also many online playgrounds that allow you to experiment with mongoDB queries without setting up a
local database instance and shell, such as [this](https://mongoplayground.net/).

<div>
However, to put what you've learnt into practice (bulding an application) later, I highly recommend getting your feet wet with the shell and local mongoDB instance first, since it will be necessary to set up your application drivers later on!
</div>
</box>


Since you can now interact with your local mongoDB instance through the shell, you could learn some of the
basic syntax and how to administrate your local mongoDB instance.

Before you start, though, it may be a good idea to reinforce some of the key terminology in mongoDB.<br>
Look under the ["Key Components of MongoDB Architecture"](https://www.guru99.com/what-is-mongodb.html) heading here if you need a refresher.

Firstly, here are some great resources on mongoDB:
- [Data-flair](https://data-flair.training/blogs/mongodb-create-database/) is a great starting point on administration of
your local mongoDB instance (creating databases, collections, etc.). Thereafter, it also goes briefly into
each topic of mongoDB.
- [MongoDB documentation](https://docs.mongodb.com/manual/crud/) can be overwhelming, but it is also
a great starting point to learn and test features of mongoDB, and is the defacto reference for it.

To guide you through your journey, here are the topics that you should peruse on the above sites _in order_.
You should first use the data-flair site to get a good hang of the topic, thereafter, going into the mongoDB documentation to further test and explore different options / queries.

1. Basic database administration (I recommend the topics on the data-flair site for this, as the mongoDB documentation can be overwhelming here)
2. CRUD operations
3. Data aggregation

After learning these core features and getting familiar with the syntax, you could try your hands at building
a simple project to get a good feel for NoSQL in an actual backend.

Depending on the backend language you are using, you should browse through the documentation [here](https://docs.mongodb.com/ecosystem/drivers/) for the appropriate language and learn how to connect to your
mongoDB instance from your application and utilize the features you learnt above.

<box type="success" light add-class="pb-1" icon-size="2x">

- Syntax for the different drivers will inevitably vary slightly from language to language. However, the core concepts stay the same.
- If you want something familiar, and you have knowledge of nodeJS, I highly recommend getting started with the
nodeJS driver which is very close to the shell syntax.
</box>

Congratulations! You should have a good handle on mongoDB by now, and learnt how you would generally interact with NoSQL databases.

If you're interested in learning more about mongoDB, I recommend going through the following topics in order in the above resources:
1. Indexes 
2. Schema validation
3. Sharding (horizontal scaling)
4. Replica sets (redundancy)

Otherwise, you could check out some other popular NoSQL databases, which can even be complementary to mongoDB.
- [Redis](https://redis.io/) - An in-memory NoSQL _key-value_ database used for caching purposes.
- [Neo4j](https://neo4j.com/) - A NoSQL _graph database_



