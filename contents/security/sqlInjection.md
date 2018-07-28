<frontmatter>
  title: SQL Injection
  footer: footer.md
  head: head.md
</frontmatter>

{{ navbar | safe }}

# SQL Injection

Author: [Lewis Koh](https://github.com/nus-cs3281/2018/blob/master/students/lewisKoh/lewisKoh-Resume.md)

## Overview

SQL (Structured Query Language) Injection is a vulnerability found in websites using databases.

SQL is a common language which is used by websites to communicate with databases. Databases can be used to store persistent data, such as usernames and passwords, sensitive account data, or other important information used by the website. Databases are usually made of many "tables" 
which are organised in rows and columns. Each row is a separate entry in the table, and each column is a specific parameter which can be used by the entry.

As an example, a table in a database may look like this:

|   UserId   |    Username   |Password|
|:----------:|:-------------:|:------:|
|      1     |     Admin     | 123456 |
|      2     |     Alice     | pw1234 |
|      3     |      ...      |   ...  |


SQL is used to interact with the database by sending "queries" which the database responds to. Some common SQL commands used include: 
* `SELECT` - retrieves information from a table
* `UPDATE` - changes information from a table
* `INSERT INTO` - adds a new entry into a table
* `DELETE FROM` - deletes information from a table

Websites combine SQL syntax with user input to query the underlying database of the website. For example, blog websites may ask for a username and password from users for them to log in to the site. This input is used to create the SQL statement that will be sent to the database. A website which is vulnerable to SQL injection attacks may construct the SQL query like this:

```sql
String query = "SELECT * FROM Users WHERE Username = ‘" + input_username +"’ 
                AND Password = ‘" + input_password + "’";
```

As an example, if a user entered their username, `Alice`, and password, `pw1234` into the website to try to gain access:

```
Username: Alice
Password: pw1234
```

The constructured query would look like this:

```sql
SELECT * FROM Users WHERE Username = ‘Alice’ 
AND Password = ‘pw1234’
```

This query would find all entries in the `Users` table in the database, and return any entries where the `Username` is `Alice` and the `Password` is `pw1234`. If the previous table was searched, it would return the details of the second row as the result. Since there was a result returned, the website would be able to tell that a legitimate username and password combination was entered since the query requires that both are matched to retrieve the data. Thus, the website would know that the user is legitimate, and the user would be allowed to log into the site. On the other hand, if no result was returned by the database, the website would know that the username and password combination does not match any of the users in the database, and would deny access to the person trying to log in.


However, are users limited to just filling in legitimate username and password strings? Some websites directly use the information given by the user without any validation, thus the users are able to write code as input for their own malicious purposes. For example, users are able to add parameters to the query.

```sql
Username: foo
Password: bar’ OR ‘’=’
```

The SQL command string built from this input would be as follows:

```sql
SELECT * FROM Users WHERE Username = ‘foo’ 
AND Password = ‘bar’ OR ‘’=’’
```

In SQL, `AND` operations are checked before `OR` operations.
This query will check the database for entries where:

```
Username = foo AND Password = bar
OR
‘’=’’
```

This query will always return true, as `‘’=’’` is always true.
As such, the query can be simplified to this:

```sql
SELECT * FROM Users
```

This will return all the rows from the `Users` table in the database, regardless of username or password entered. If used in user authentication, the user will likely be able to gain access to the system.


## Preventing SQL Injection

There are a couple of ways to protect your website against SQL injection attacks. The two most common ways are:
1. Prepared Statements using Parameterized Queries
1. Whitelist Input Validation

The sections below explain the two options in more detail.

### Solution 1: Prepared Statements using Parameterized Queries

By defining all the SQL code first and passing in the parameters afterwards, you can make the database distinguish the difference between code and data. It would treat the values entered by the user as a parameter, and would not allow it to alter the query being executed. The way to achieve this varies by language, but it is easy to implement and effective.

For example, instead of writing this in Java:

```java
String query = "SELECT * FROM Users WHERE Username = " + input_username +"’ 
                AND Password = ‘" + input_password + "’";
Statement statement = connection.createStatement();
ResultSet results = statement.executeQuery( query );
```

You can prepare the statement like this:

```java
 String query = "SELECT * FROM Users WHERE Username = ? 
                AND Password = ? ";
 PreparedStatement pstmt = connection.prepareStatement( query );
 pstmt.setString( 1, input_username ); 
 pstmt.setString( 2, input_password );
 ResultSet results = pstmt.executeQuery( );
```

The `?` in the query string is a placeholder for a string value. In the example, the first `?` is substituted with the value of `input_username` using the call `pstmt.setString(1, input_username)`. If the earlier attack was attempted, The query being submitted to the database will look like this:

```sql
SELECT * FROM Users WHERE Username = 'foo' AND Password = 'bar' OR ‘’=’’
```

However, since it is treated as a value to be used, the database will not allow it to modify the query, and it would not be able to affect the structure of the SQL statement. As such, the query will check the database for entries where:

```
Username = foo
AND 
Password = bar OR ‘’=’’
```

As such, the query is safe from SQL code being injected by users.


### Solution 2: Whitelist Input Validation

By applying a whitelist to the values a user is allowed to use, you can remove undesired symbols in the query being passed to the database 
(e.g. whitelisting only alphanumeric characters for a username). This ensures that attackers won't be allowed to enter special characters 
which may have unwanted effects when executed. (e.g. special characters in SQL such as `'`, `@`, `^` and `_`)


## Resources

References:

1. https://www.owasp.org/index.php/SQL_Injection
(Basic description of SQL Injection attack taken from here)
1. http://projects.webappsec.org/f/WASC-TC-v2_0.pdf (page 105-110)
(Description of the two types of SQL injection attack taken from here)

Additional Reading Resources:

1. http://www.sqlinjection.net/
(Good starting point for diving deeper into SQL injection)
1. https://www.owasp.org/index.php/Testing_for_SQL_Injection_(OTG-INPVAL-005)
(How to test your website for SQL injection attacks)
1. https://www.owasp.org/index.php/Query_Parameterization_Cheat_Sheet
(Parameterized query examples for some common languages)
1. https://www.owasp.org/index.php/SQL_Injection_Prevention_Cheat_Sheet
(A list of possible preventions, which contains even more ways to protect your site from SQL injection attacks).
1. http://guides.rubyonrails.org/security.html#sql-injection
(Discussion of how SQL injection attacks work, some possible scenarios of such attacks, and possible preventive measures using Ruby on Rails)
1. https://www.owasp.org/index.php/Blind_SQL_Injection
(Discussion about a type of SQL injection attack, when only generic messages are displayed by the website)
1. https://cwe.mitre.org/data/definitions/89.html
(Comprehensive coverage of the aspects of SQL injection)

Additional Resources:

1. https://github.com/sqlmapproject/sqlmap
(Penetration testing tool that detects and exploits SQL injection flaws)
1. https://find-sec-bugs.github.io/
(Plugin to detect security vulnerabilities in Java web applications, including SQL injection)