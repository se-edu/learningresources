<frontmatter>
  title: Introduction to SQL
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Introduction to SQL

**Author(s): [Amrut Prabhu](https://github.com/amrut-prabhu)**

Reviewers: [Ronak Lakhotia](https://github.com/RonakLakhotia), [Rahul Rajesh](https://github.com/rrtheonlyone)

## What is SQL?

Databases are integral to any commercial software application, whether it is a web app, desktop app or otherwise.
As the size of your application grows, you need to look into dedicated database software packages (like SQL software) to store your data. It will not be enough to simply rely on in-memory solutions or a simple text file to store the data. Many companies like Facebook, Google, Adobe, Paytm and Zappos use <tooltip content="Relational DataBase Management System">RDBMS</tooltip> software to fulfil this need.

**Structured Query Language** (**SQL**) is a programming language that is specifically designed for managing (storing, retrieving and manipulating) data in a <tooltip content="relational databases store data in tables">[relational database]({{baseUrl}}/contents/data/databases/databases.html#database-models)</tooltip>. Unlike other languages, SQL doesn't come as a standalone installation. Rather, _SQL Software_ (or _SQL Implementations_) like MySQL, Oracle, SQL Server and PostgreSQL come with a RDBMS that uses the SQL language.
However, the syntaxes and functions used across these software systems <tooltip content="in terms of whether the syntax is case-sensitive, the format for specifying date and time, what functions are available out-of-the-box">vary</tooltip>.

<box type="tip">
  Many online resources refer to Microsoft SQL Server as SQL. So, keep that in mind before you start making conclusions about the characteristics and features of SQL, the language.
</box>

---

### How does SQL work?

SQL uses **queries** to retrieve data. Here is an example of how an SQL query is used.

Suppose we have the data table `Students` shown here:

| ID | Name  | Course | Faculty |
| -- | ----- | ------ | ------- |
| 1  | Alex  | CS202  | CS      |
| 2  | Bob   | MA303  | MA      |
| 3  | Cathy | CS202  | CS      |
| 4  | Daren | CS202  | CS      |
| 5  | Ellie | CS101  | CS      |
| 6  | Fred  | MA303  | MA      |
| 7  | Gary  | CS101  | CS      |
| 8  | Henry | CS404  | CS      |

We can use this SQL query to retrieve information from this table:
```
SELECT Course, COUNT(*) num
FROM Students
WHERE Faculty = 'CS'
GROUP BY Course
HAVING COUNT(*) > 1
ORDER BY num;
```

<box type="tip">
  The uppercase words in this query are either **clauses** or **statements**, and are SQL keywords.
</box>

This query first filters the entries in the `Students` table such that only entries that have `CS` as the faculty are considered.
After that, it groups those entries into the 3 courses: `CS101`, `CS202` and `CS404`.
Then, it removes courses that do not have more than 1 student, i.e., `CS404` is removed from consideration.
Finally, it returns a list of courses with a count of the number of students, ordered in increasing order.
So, the output of the query is:

| Course | num |
| ------ | --- |
| CS202  | 3   |
| CS101  | 2   |

<box type="info">
  You can experiment with this example on <a href="https://www.db-fiddle.com/f/kHqV2edUGxCc1dU6vE6CmS/0">DB Fiddle</a> by entering SQL queries and then running them.
</box>

You can see how this simple query can prove to be extremely useful for getting this information when the table has tens of entries (or many more). Apart from retrieving information, SQL can also be used for creating, deleting and manipulating data with queries like  `INSERT`, `DELETE` and `UPDATE` for entries, in addition to `CREATE`, `DROP` and `ALTER` for tables as a whole.

---

## Why learn SQL?

Now that you have seen how SQL is used, let's take a look at some of the main reasons behind the widespread adoption of SQL and why you should learn it.

### Easy to learn

As you saw in the example in the previous section, SQL is not really that complex. It is easy to learn, even for beginners who do not have any prior experience with databases. Since it has been around for a few decades, there are many good books and online resources to learn from.
In addition, SQL (and SQL software) has a huge support community (such as [Stack Overflow](https://stackoverflow.com/questions/tagged/sql), and the [official MySQL forum](https://forums.mysql.com/)) which can prove useful when you run into problems while using it.

### Free

One of the benefits (and reasons) for SQL's popularity is that there is free software available (%like MySQL, PostgreSQL and SQLite%) as well as paid ones (%%like Microsoft SQL Server and Oracle%%) that come with more functionality.

One such example is MySQL. It is free of cost and comes with the official [MySQL Workbench](https://dev.mysql.com/doc/workbench/en/) as well as other third party easy-to-use <tooltip content="Graphical User Interface">GUIs</tooltip>, which are less daunting to new users as compared to a <tooltip content="Command Line Interface">CLI</tooltip>.

### Highly Compatible

SQL software works on many operating systems (%%like Windows, Mac OS, Ubuntu, Red Hat among others%%) and more importantly, can be integrated with many languages. This includes programming languages like C++, Ruby, Java, Python, JavaScript (Node.js), and Go.

In addition, online playgrounds like [DB Fiddle](https://www.db-fiddle.com/) and [JDoodle](https://www.jdoodle.com/execute-sql-online) make it easy to use and learn the SQL language quickly without the hassle of setting up an environment or application.

## Disadvantages

SQL is not without its problems. In general, the biggest problem is with regards to the features of SQL.

Although SQL databases use established <tooltip content="American National Standard Institutes">ANSI</tooltip> & <tooltip content="International Organization for Standardization">ISO</tooltip> standards, some SQL software systems (%%PostgreSQL, for example%%) add proprietary extensions to standard SQL.
Due to this, the available feature set can vary according to what you're using. This can make SQL confusing and frustrating to use when switching SQL software.

Apart from that, most **SQL** problems are not uniformly applicable across all SQL software. For example, MySQL suffers from concurrency issues. Though it performs well with read operations, it *can* be problematic when there are many concurrent read-write operations. A symptom of this issue would be a sudden slowdown of a query. However, PostgreSQL deals with concurrency well by implementing <tooltip content="each query transaction is isolated from other transactions running simultaneously in the system">transactions isolation</tooltip>.

---

## How to get started with SQL?

The first thing to do when getting started with SQL is to choose a SQL software. You can either just use an online SQL playground or install a dedicated application. Our suggestion is to use either MySQL or SQLite online.

SQL has been around for a long time and hence, you can find really good books and resources to learn it well. However, these resources can be overwhelming. So, you can decide which parts you want to learn, as you will likely not need to know everything to get started with using a SQL database.

Here are some recommended steps for learning SQL:

- Understand what a database and [<tooltip content="DataBase Management System">DBMS</tooltip>]({{baseUrl}}/contents/data/databases/databases.html) are. More specifically, understand the basic concepts of [RDBMS](https://www.tutorialspoint.com/sql/sql-rdbms-concepts.htm). This is not _essential_, but will give you a better high-level understanding before diving into programming.

- Khan Academy offers a very good free [SQL course](https://www.khanacademy.org/computing/computer-programming/sql). It uses a mix of videos, text, and exercises that you can choose to do at your own pace. It is well organized and even provides summary notes.

- W3Schools offers an interactive written tutorial for [SQL](https://www.w3schools.com/sql/default.asp) in which you can run SQL queries to see the examples in action.

- Once you're familiar with the language, you can make use of a [SQL Quick Reference Guide](https://www.w3schools.com/sql/sql_ref_mysql.asp) to look up information quickly.

- You can also refer to [40 important SQL queries](https://bytescout.com/blog/20-important-sql-queries.html) to get a quick overview (or refresher) of SQL.

---

## Where to go from here?

Look into different SQL software distributions and decide which one you want to work with. You can start by comparing the 3 mentioned in this [Udemy post](https://blog.udemy.com/oracle-vs-mysql-vs-sql-server/) (and in [DB-Engines](https://db-engines.com/en/system/Microsoft+SQL+Server%3BMySQL%3BOracle)') and work from there. Then, try to set up the RDBMS software and integrate it with an application. This will give you good experience and exposure to how databases are used in practice.

For example:

  - [Guru99](https://www.guru99.com/introduction-to-database-sql.html) has a good DBMS guide that uses MySQL. It covers the entire learning process, from basic database concepts to setting up and using MySQL.

  - Integrate a database (MySQl, PostgreSQL etc.) with your preferred language. For MySQL, you can use the [mysql](http://www.mysqltutorial.org/mysql-nodejs/) module for Node.js, [JDBC](http://www.mysqltutorial.org/mysql-jdbc-tutorial/) for Java, and [MySQL Connector](http://www.mysqltutorial.org/python-mysql/) for Python.

  - W3Schools have interactive written tutorials such as [MySQL Node.js](https://www.w3schools.com/nodejs/nodejs_mysql.asp) and [MySQL Python](https://www.w3schools.com/python/python_mysql_getstarted.asp), for learning how to integrate MySQL with Node.js and Python respectively.
