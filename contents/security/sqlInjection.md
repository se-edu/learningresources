<frontmatter>
  title: SQL Injection
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# SQL Injection

**Authors: [Jiang Chunhui](https://github.com/Adoby7), [Lewis Koh](https://github.com/nus-cs3281/2018/blob/master/students/lewisKoh/lewisKoh-Resume.md)**

Reviewers: [Chattoraj Ayush](https://github.com/AyushChatto), [Monika Manuela Hengki](https://github.com/monmanuela),  [Nicholas Chua](https://github.com/nicholaschuayunzhi), [Rachael Sim](https://github.com/rachx), [Tran Tien Dat](https://github.com/tran-tien-dat), [Wen Xin](https://github.com/wenmogu)

<box id="article-toc">

* [SQL‎](#sql)
* [How does SQL Injection Work?‎](#how-does-sql-injection-work)
* [Why is it Important to Prevent SQL Injection?‎](#why-is-it-important-to-prevent-sql-injection)
* [Preventing SQL Injection‎](#preventing-sql-injection)
  * [Solution 1: Prepared Statements using Parameterized Queries‎](#solution-1-prepared-statements-using-parameterized-queries)
  * [Solution 2: Whitelist Input Validation‎](#solution-2-whitelist-input-validation)
* [Resources‎](#resources)
</box>

## SQL
SQL (Structured Query Language) is a common language which is used by websites to communicate with databases. Databases can be used to store persistent data, such as usernames and passwords, sensitive account data, or other important information used by the website. Typically, SQL works on relational databases, which are usually made of many "tables" organised in rows and columns. Each row is a separate entry in the table, and each column is a specific parameter which can be used by the entry. A sample table is shown below:

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
* `DROP` - deletes the whole table

In addition, a query could use parameters to filter, reorder, and group the returned results. For example, the following query will only returns the records in table "users" whose user name is "Admin". Here the parameter `Username = Admin` in the `WHERE clause` works as a filter.
```SQL
SELECT * FROM users WHERE Username = 'Admin'
```


More information about SQL can be found [here](https://www.w3schools.com/sql/).

## How does SQL Injection Work?

>**SQL injection is the placement of malicious code in SQL statements, via web page input.**<br>--source: [w3schools](https://www.w3schools.com/sql/sql_injection.asp)

To learn about SQL injection, let us suppose that a typical website connects to a database which stores user information like below:

|   UserId   |    Username   |Password|
|:----------:|:-------------:|:------:|
|      1     |     Admin     | 123456 |
|      2     |     Alice     | pw1234 |
|      3     |      ...      |   ...  |

Then the website prompts a login form to require the user to enter `username` and `password`. After receiving the data, it generates the following SQL query: 

```sql
String query = "SELECT * FROM Users WHERE Username = ‘" + input_username +"’ 
                AND Password = ‘" + input_password + "’";
```

 Next, the website checks whether the query returns any record to verify whether the user enters the correct password.

As an example, if a user entered their username, `Alice`, and password, `pw1234` into the website to try to gain access:

```
Username: Alice
Password: pw1234
```

The constructed query would look like this:

```sql
SELECT * FROM Users WHERE Username = ‘Alice’ 
AND Password = ‘pw1234’
```

This query would find all entries in the `Users` table in the database, and return any entries where the `Username` is `Alice` and the `Password` is `pw1234`. If the previous table was searched, it would return the details of the second row as the result. Since there was a result returned, the website would be able to tell that a legitimate username and password combination was entered since the query requires that both are matched to retrieve the data. Thus, the website would know that the user is legitimate, and the user would be allowed to log into the site.
 On the other hand, if no result was returned by the database, the website would know that the username and password combination does not match any of the users in the database, and would deny access to the person trying to log in.


**However, some websites may not check the syntax of user input rigorously, and therefore a malicious user can inject SQL query via the user input.** 

In the example above, the website directly substitutes the information given by the user without any validation. In this case, an attacker can supply some malicious SQL code in the user input such that it changes the nature of the SQL statement executed.

 For example, the attacker can add more parameters to the query:

```sql
Username: Admin
Password: foo’ OR ‘1’=‘1
```
The SQL command string built from this input would be as follows:

```sql
SELECT * FROM Users WHERE Username = ‘Admin’ 
AND Password = ‘foo’ OR ‘1’=‘1’
```

In SQL, `AND` operations are checked before `OR` operations.
This query will check the database for entries where:

```
(Username = Admin AND Password = foo) OR (‘1’=‘1’)
```

This `where` clause will always return true, as `‘1’=‘1’` is always true.
As such, the query can be simplified to this:

```sql
SELECT * FROM Users
```

This will return all the rows from the `Users` table in the database, regardless of username or password entered. 

The above technique of injecting malicious SQL code via user input is called SQL injection. If used in user authentication, the attacker is able to gain access to anyone's account. Moreover, this attacker can also modify sensitive information if the account owner has the privilege (e.g. a lecturer who can modify students' marks).


**In addition to adding extra parameters to compromise the authentication, a malicious user may even add custom queries to view, modify the records in database, or even delete the whole database.**

An SQL query ends with a semicolon ";". In the previous section the malicious user terminates one parameter by single quote "'", and add more parameters behind it. Now, he can also terminate the query by semicolon, and adds another query at the back:

```sql
Username: foo
Password: bar’; DROP TABLE Users;
```

```sql
SELECT * FROM Users WHERE Username = ‘foo’ AND Password = ‘bar’; 
DROP TABLE Users;
```

When the database executes these two queries, it will delete all user information. Then other users cannot access this website. In addition to the `DROP` query, the attacker may also inject `SELECT` and `INSERT` queries, which can either read sensitive data from database or add data to it. 

## Why is it Important to Prevent SQL Injection?

1. **It is the most common type of attack.** According to [Open Web Application Security Project (OWASP) report](https://www.owasp.org/images/7/72/OWASP_Top_10-2017_%28en%29.pdf.pdf), the injection attack is always the annual top 1 application security risk from 2013 to 2017. In addition, [Statistics from Akamai](https://www.akamai.com/uk/en/resources/our-thinking/state-of-the-internet-report/web-attack-visualization.jsp) shows that in one week, over 80% of attacks are SQL injection.
1. **It can have serious consequences.** SQL injection can cause the loss of large amount of money. Based on the [Global Threat Intelligence Report](https://www.helpnetsecurity.com/2014/03/28/analysis-of-three-billion-attacks-reveals-sql-injections-cost-196000/), even a small SQL injection attack may cause hundreds of thousands dollars lost. 
Furthermore, information disclosure is another serious consequence. [An SQL injection attack on the toymaker company, VTech](https://coar.risc.anl.gov/consequences-of-sql-injection-attacks/), caused millions of parents' and children's profiles to be stolen. 
Thus, we need to pay attention to prevent this attack in our code.
1. **It is easy to prevent.** Referring to the section below, you do not need to put too many efforts on preventing from SQL injection. As it can prevent such a common attack, why not do it now?

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
</div>
