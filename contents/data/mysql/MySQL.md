<frontmatter>
  title: Introduction to MySQL
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Introduction to MySQL

Author(s): [Amrut Prabhu](https://github.com/amrut-prabhu)

## What is MySQL?

<tooltip content="My Structured Query Language(SQL)">MySQL</tooltip> is a free and open source database software that is currently sponsored and controlled by the [Oracle Corporation](https://www.zdnet.com/article/mysql-why-the-open-source-database-is-better-off-under-oracle/). Technically, it is a <tooltip content="Relational DataBase Management System">**RDBMS**</tooltip> that is based on the <tooltip content="Structured Query Language">SQL</tooltip> language and is widely used in many small and big companies. According to their [official website](https://www.mysql.com/why-mysql/), this includes companies like Facebook, Google, Adobe, Paytm and Zappos (though they may not be using MySQL exclusively).

<box type="tip">
	SQL is a standard language for relational database operations- storing, retrieving and updating data. There are several database platforms that use SQL (such as MySQL, PostgreSQL, Microsoft SQL Server and more), but each has a slightly different syntax.
</box>

---

## Why learn MySQL?

Being open source and one of the first DBMSs, MySQL rose to popularity in its early days. It ended up being a core component of the <tooltip content="Linux OS, Apache Server, MySQL RDBMS, PHP  language">LAMP</tooltip> web application stack (and many others).
Here are some of the main reasons behind the widespread adoption of MySQL and why it still remains popular today.

### Easy to learn
MySQL is very easy to learn, even for beginners who do not have any prior experience with databases. Since it has been around for well over a decade, there are many good books and online resources to learn from.
In addition, it has a huge support community (such as the [official forum](https://forums.mysql.com/) and [Stack Overflow](https://stackoverflow.com/questions/tagged/mysql)) which can prove useful when you run into problems while using MySQL.

### Free of cost

Since it is an open source software, you do not have to pay to use MySQL. It even comes with official (MySQL Workbench) as well third party easy-to-use <tooltip content="Graphical User Interface">GUIs</tooltip>, which are less daunting to new users as compared to a <tooltip content="Command Line Interface">CLI</tooltip>.

### Compatibility

MySQL works on many operating systems and more importantly, with many languages. These include languages from PHP, PERL, C++, Ruby, Java to *relatively* newer ones like Python, JavaScript (Node.js) and Go.

### Secure

MySQL ensures data security in terms of data backup and data protection.  As compared to NoSQL databases, MySQL (and relational databases) provide better guarantees for data consistency. In terms of security, unauthorized access to data is prevented through encryption and user blocking capabilities, and safer connections are possible through SSH and SSL.

### Disadvantages

At the same time, MySQL is not without its problems:

- Scalability: MySQL is not known. It may be better to look into alternatives like [NoSQL](httpss://)

- Features:
MySQL can lack features such as full outer joins and full-text search, depending on the database engine. As a consequence of being simple, MySQL is not as powerful as something like PostgreSQL, for example, which has a lot more features and customizability.

- Concurrency:
Though storage engines like MySQL perform well with read operations, it *can* be problematic when there are concurrent read-write operations. A symptom of this concurrency issue would be a sudden slowdown of a well-optimized query.

---

## How does MySQL work?

## How to get started with MySQL?

## Where to go from here?

- [Guru99](https://www.guru99.com/introduction-to-database-sql.html) has a good guide for the entire process, from basic concepts to setting up and learning MySQL.

- You can always refer to the [official guide](https://dev.mysql.com/doc/refman/8.0/en/tutorial.html) to get a more comprehensive understanding of MySQL and its capabilities.

- W3Schools offers interactive tutorials for [SQL](https://www.w3schools.com/sql/default.asp)(and MySQL), [MySQL Node.js](https://www.w3schools.com/nodejs/nodejs_mysql.asp), [MySQL Python](https://www.w3schools.com/python/python_mysql_getstarted.asp)

- You can also use Quick Reference guides to just look up information quickly once you're familiar with the language:
  - [SQL Quick Reference](https://www.w3schools.com/sql/sql_ref_mysql.asp)
  - There is a section for MySQL Datatypes [here]](https://www.w3schools.com/sql/sql_datatypes.asp)
  - [MySQL functions](https://www.w3schools.com/sql/sql_ref_mysql.asp)

---

## References

- [Official MySQL website](https://www.mysql.com/)
- [Why MySQL is popular](https://www.fromdev.com/2017/03/what-is-mysql-why-is-it-so-popular.html)
- [Reasons and challenges for choosing MySQL](https://dataconomy.com/2017/04/5-reasons-challenges-mysql/)
