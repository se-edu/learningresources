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

Author(s): [Amrut Prabhu](https://github.com/amrut-prabhu)

## What is SQL?

Databases are integral to any commercial software application, and most student side projects as well, whether it is a web app, desktop app or otherwise.
Though you can choose to just use a text file to store your data, this will not be sustainable as the size of your application (and the data stored) grows. This is why there are several dedicated database software packages available today.

**SQL** is a programming language that is specifically designed for managing (storing, querying and manipulating) data in a <tooltip content="relational databases store data in tables">[relational database]({{baseUrl}}/contents/data/databases/databases.htmll#database-models)</tooltip>. As mentioned in [Codecademy](https://www.codecademy.com/articles/what-is-rdbms-sql), most <tooltip content="Relational DataBase Management System">RDBMSs</tooltip> like MySQL, Oracle, SQL Server and PostgreSQL use the SQL language. However, the syntaxes used in these distributions vary slightly. These differences may be in terms of case-sensitivity, available built-in functions, custom functions, date and time formats, and so on.  

SQL distributions are widely used in many small and big companies. According to the official [MySQL website](https://www.mysql.com/why-mysql/), it is used in companies like Facebook, Google, Adobe, Paytm and Zappos (though they may not be using MySQL exclusively).

<box type="tip">
  Many online resources refer to Microsoft SQL Server as SQL. So, keep that in mind before you start making conclusions about the characteristics and features of SQL, the language.
</box>

---

## Why learn SQL?

Being open source and one of the first DBMSs, MySQL (a specific implementation of SQL) increased the popularity of **SQL** in its early days. It ended up being a core component of the <tooltip content="Linux OS, Apache Server, MySQL RDBMS, PHP  language">LAMP</tooltip> web application stack (and many others).
Here are some of the main reasons behind the widespread adoption of SQL and why it remains popular today.

### Easy to learn

SQL is easy to learn even for beginners who do not have any prior experience with databases. Since it has been around for a few decades, there are many good books and online resources to learn from.
In addition, SQL and its distributions have a huge support community (such as [Stack Overflow](https://stackoverflow.com/questions/tagged/sql), and the [official MySQL forum](https://forums.mysql.com/)) which can prove useful when you run into problems while using SQL.

### Free of cost

One of the benefits (and reasons) for SQL's popularity is that there are free distributions available (like MySQL, PostgreSQL and SQLite) as well as paid ones (like Microsoft SQL Server and Oracle) that come with more functionality.

One such distribution is MySQL. It is free of cost and comes with official ([MySQL Workbench](https://dev.mysql.com/doc/workbench/en/)) as well third party easy-to-use <tooltip content="Graphical User Interface">GUIs</tooltip>, which are less daunting to new users as compared to a <tooltip content="Command Line Interface">CLI</tooltip>.

### Compatibility

SQL distributions work on many operating systems and more importantly, can be integrated with many languages. This includes languages like PHP, PERL, C++, Ruby, Java as well as *relatively "newer"* ones like Python, JavaScript (Node.js) and Go.

In addition, online playgrounds like [DB Fiddle](https://www.db-fiddle.com/) and [JDoodle](https://www.jdoodle.com/execute-sql-online) make it easy to use and learn the SQL language quickly without the hassle of setting up an environment or application.

## Disadvantages

SQL is not without its problems. In general, the biggest problem is with regards to the features of SQL.

Although SQL databases use established <tooltip content="American National Standard Institutes">ANSI</tooltip> & <tooltip content="International Organization for Standardization">ISO</tooltip> standards, some distributions (PostgreSQL, for example) add proprietary extensions to the standard SQL to ensure customer lock-in.
Thus, the available feature set varies according to the SQL distribution that you are using. This can make it confusing and frustrating to use SQL when switching across distributions.

Apart from that, most **SQL** problems are not uniformly applicable to all of its distributions.

For example, **MySQL** suffers from concurrency issues. Though it performs well with read operations, it *can* be problematic when there are many concurrent read-write operations. A symptom of this issue would be a sudden slowdown of a well-optimized query. However, **PostgreSQL** deals with concurrency well by being fully <tooltip content="Atomicity, Consistency, Isolation, Durability properties are satisfied">ACID</tooltip> compliant and implementing <tooltip content="transaction must be Isolated from other concurrent transactions running in the system">transactions isolation</tooltip>.

---

## How does SQL work?

Let's look at what a basic **SQL query** (or "command") for retrieving information from a data table looks like:

```
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
HAVING condition
ORDER BY column_name(s);
```

<box type="tip">
  The uppercase words in this query are clauses or statements, and are SQL keywords.
</box>

### Breaking down the clauses

Let's understand what the above query means by using an example to make it more concrete.

Suppose have the data table `Students` shown here. We want to get a list of CS courses that have more than 1 student. The output should display the Course name and number of students, and should be sorted by increasing number of students.

| ID | Name  | Course |
| -- | ----- | ------ |
| 1  | Alex  | CS202  |
| 2  | Bob   | MA303  |
| 3  | Cathy | CS202  |
| 4  | Daren | CS202  |
| 5  | Ellie | CS101  |
| 6  | Fred  | MA303  |
| 7  | Gary  | CS101  |
| 8  | Henry | CS404  |

Table 1. `Students` table

From the data in our table above, entries 2, 6 and 8 should not be considered in the output as they do not meet the criteria. So, the expected output is:

| Course | num |
| ------ | --- |
| CS101  | 2   |
| CS202  | 3   |

Table 2. Expected output

The corresponding query will be:

```
SELECT Course, COUNT(*) num
FROM Students
WHERE Course LIKE 'CS%'
GROUP BY Course
HAVING COUNT(*) > 1
ORDER BY num;
```

An important thing to note here is that queries aren't executed from top to bottom. This example query actually follows this logical order of execution:
- `FROM` clause
- `WHERE` clause
- `GROUP BY` clause
- `HAVING` clause
- `SELECT` statement
- `ORDER BY` clause

As you can see, the `FROM` clause is processed first while the `SELECT` clause which appears at the start is processed much later.

Now, let's go through each clause in the query, in the order that they are executed.

#### 1. `FROM Students`

This clause means that we use the `Students` table as the data source for our query.
The <tooltip content="See <code>5. SELECT ... </code> below">result-set</tooltip> still looks the same as the original `Students` table (Table 1).

#### 2. `WHERE Course LIKE 'CS%'`

This is a conditional clause. `LIKE` is a SQL keyword that is used for pattern matching. The `%` means any string of any length.

So, in this clause, we are filtering the `Student` table rows such that the row's `Course` has a prefix `CS`. Thus, entries 2 and 6 are removed from consideration. Now, our result-set looks like this:

| ID | Name  | Course |
| -- | ----- | ------ |
| 1  | Alex  | CS202  |
| 3  | Cathy | CS202  |
| 4  | Daren | CS202  |
| 5  | Ellie | CS101  |
| 7  | Gary  | CS101  |
| 8  | Henry | CS404  |
Table 3. Filtered result after `WHERE`

#### 3. `GROUP BY Course`

This is used to group the result set, and is often used with aggregate functions (like `COUNT`, `MAX`, `AVG` etc.).

In this query, we are essentially grouping into 3 "groups": `CS101` group (IDs 5 and 7), `CS202` group (IDs  1, 3, and 4), and `CS404` group (ID 8).

#### 4. `HAVING COUNT(*) > 1`
This is a conditional clause that is used with aggregate functions. `COUNT(*)` is an aggregate function that returns the number of rows in each "group".

So, this clause will filter out the `CS404` group since it has a count of 1, and doesn't satisfy the condition.

#### 5. `SELECT Course, COUNT(*) num`
The `SELECT` statement is used for choosing the data to display. The returned data is stored in a result table, called the **result-set**.

In this statement, we choose the `Course` and its count, `num` to be the result-set that is to be displayed. Adding `num` after `COUNT(*)` means that it is an alias for `COUNT(*)`. The result-set will look like this:

| Course | num |
| ------ | --- |
| CS202  | 3   |
| CS101  | 2   |
Table 4. Result-set after `SELECT`

<box type="info">
  Since, we have used a <code>GROUP BY</code> clause, we cannot <code>SELECT</code> data from an individual attribute (like ID or Name) that is not part of the aggregate data generated in the group (like Course).
</box>

#### 6. `ORDER BY num`

Finally, we need to sort our result entries. We use the `ORDER BY` clause (which sorts in ascending order by default) for doing so.

In this clause, we order the result-set in ascending order based on the `num` attribute (column). So, our final output is:

| Course | num |
| ------ | --- |
| CS101  | 2   |
| CS202  | 3   |
Table 4. Final result after `ORDER BY`


This is the same as our expected output from Table 2!

<box type="info">
  You can experiment with this example on <a href="https://www.db-fiddle.com/f/yxjjgbkKmsa46cKjeEg1X/2">DB Fiddle</a> by entering queries into the <code>Query SQL</code> pane and then clicking the <code>Run</code> icon.
</box>

The example shown here is relatively simple. Typical SQL queries have the capability to be much more complex as there are a lot of clauses, functions, and operators that are not covered here. As the complexity of the query grows, there is a possible decrease in the performance of the query. This is why [query planning](https://www.khanacademy.org/computing/computer-programming/sql/relational-queries-in-sql/a/more-efficient-sql-with-query-planning-and-optimization) and [optimization](https://www.sisense.com/blog/8-ways-fine-tune-sql-queries-production-databases/) is important.

---

## How to get started with SQL?

The first thing to do is to choose a SQL distribution. You can either just use an online SQL playground or install a dedicated application. My suggestion would be to use either MySQL or SQLite online.

SQL has been around for a long time and hence, you can find really good books and resources to learn it well. However, these resources can be overwhelming. So, you can decide which parts you want to learn as you will likely not need to know everything to get started with using a SQL database.

Here are some recommended steps that you can use to start learning SQL:

- Understand what a database and [<tooltip content="DataBase Management System">DBMS</tooltip>]({{baseUrl}}/contents/data/databases/databases.html) are. More specifically, understand the basic concepts of [RDBMS](https://www.tutorialspoint.com/sql/sql-rdbms-concepts.htm). This is not _essential_, but will give you a better high-level understanding before diving into programming.

- Khan Academy offers a very good [SQL course](https://www.khanacademy.org/computing/computer-programming/sql) for free. It has a mix of videos, text, and exercises that you can choose to do at your own pace. It is well organized and even provides summary notes.

- W3Schools offers an interactive written tutorial for [SQL](https://www.w3schools.com/sql/default.asp).

- You can also use a [SQL Quick Reference Guide](https://www.w3schools.com/sql/sql_ref_mysql.asp) to look up information quickly once you're familiar with the language.

---

## Where to go from here?

Look into SQL distributions and decide which one you want to work with. You can start by comparing the 3 distributions mentioned [here](https://blog.udemy.com/oracle-vs-mysql-vs-sql-server/) (and [here](https://db-engines.com/en/system/Microsoft+SQL+Server%3BMySQL%3BOracle)) and work from there. Then, try to set up a RDBMS and integrate it with an application. This will give you good experience and exposure to how databases are used in practice.

For example:

  - [Guru99](https://www.guru99.com/introduction-to-database-sql.html) has a good DBMS guide that uses MySQL. It covers the entire learning process, from basic database concepts to setting up and using MySQL.

  - Integrate a database (MySQl, PostgreSQL etc.) with your preferred language. For MySQL, you can use the [mysql](http://www.mysqltutorial.org/mysql-nodejs/) module for Node.js, [JDBC](http://www.mysqltutorial.org/mysql-jdbc-tutorial/) for Java, and [MySQL Connector](http://www.mysqltutorial.org/python-mysql/) for Python.

  - W3Schools have interactive written tutorials such as [MySQL Node.js](https://www.w3schools.com/nodejs/nodejs_mysql.asp) and [MySQL Python](https://www.w3schools.com/python/python_mysql_getstarted.asp), for learning how to integrate MySQL with Node.js and Python respectively.

---

## References

- [SQL capabilities](https://www.databasejournal.com/sqletc/article.php/3915331/Top-10-SQL-Hierarchical-Data-Processing-Capabilities.htm)
- [Why MySQL is popular](https://www.fromdev.com/2017/03/what-is-mysql-why-is-it-so-popular.html)
- [Advantages and disadvantages of SQL](https://www.quora.com/What-are-the-advantages-and-disadvantages-of-SQL)
- [MySQL and PostgreSQL differences](http://mwiki.gichd.org/IM/Difference_MySQL_PostGreSQL)
- [Execution order of SQL query](https://www.designcise.com/web/tutorial/what-is-the-order-of-execution-of-an-sql-query)
