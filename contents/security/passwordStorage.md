<frontmatter>
  title: Introduction to Password Storage
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Introduction to Password Storage

Author: [Jeremy Choo](https://github.com/ChooJeremy)

## Overview

While developing your own application, it is very common to implement login systems to be able to identify users. Whenever there is a need to store user-specific information, developers typically implement user accounts to identify these users separately. You might have seen ways this are done in other websites - using registration forms and requring you to login with a username and password to identify you for the session.

As a developer, storing these usernames and passwords are essential to be able to refer to them later. While there are many ways to store these data - from text files to databases - this article will focus more on how to store passwords properly. For the purpose of this article, we will assume that you are storing the passwords in a database. Modern password mechanism typically involve **salting** and **hashing** the password. This article will describe what these mean and why they are important.

### What's wrong with <trigger for="pop:plaintext">plaintext</trigger>?

<popover id="pop:plaintext" title="_Plaintext_ refers to unencrypted information" placement="top">
  <div slot="content">
It is a cryptography term generally referring to text before encyption or after decrypting it. Another term for it is cleartext.
  </div>
</popover>

While usernames are typically public information, thus encrypting them is not as important, passwords are typically secret information and require encryption. If passwords are stored in plaintext and your server's database is ever compromised, attackers can easily use the stored plaintext passwords to login as the user and potentially access other functionality. This grants the attacker more power than just reading values off the database and being unable to read them if they were encrypted.

In addition,
* Your employees can see the passwords of your users. If your employees are ever compromised, they might be willing to simply leak the password.
* It is common for users to re-use the same password on multiple sites. If your passwords are stored as plaintext and accessed, attackers can easily re-use the same passwords on other websites (such as banking websites) and gain access to those.

To prevent all of these measures, it is good to perform hashing on passwords. Unlike encryption, hashing is a <trigger for="pop:oneway">one-way function</trigger>. This means that an attacker cannot decrypt the encrypted password to retrieve the original password, thus ensuring that all of the above vulnerabilities isn't possible.

<popover id="pop:oneway" title="A one way function is a function that is easy to compute in one way, but is close to impossible to compute the other way." placement="top">
<div slot="content"></div>
</popover>

## What is hashing?

Hashing is the act of transforming a set of data into another set of data that is difficult to reverse. As an example, suppose you wanted to multiply 2 prime numbers together, such as 17 and 11. This is easy for a computer to calculate, and it results in 187. However, suppose you were given a number that is the result of 2 prime numbers - such as 299. How do you find out which 2 prime numbers multipled together to create 299?

It definitely isn't 2, because the number isn't even.  
It isn't 3, because the digits don't sum to a multiple of 3.  
It isn't 5, because it doesn't end in a 5 or a 0.  
Is it 7? Well, it's hard to tell, you would have to start doing division to test.  
 
There are a few shortcuts that computers and mathematicans can use to make the operation faster, but overall it is a long process that isn't efficient at all, particularly for huge numbers. Thus, it is easy to perform the function one way, but hard to reverse. 

Computers use a similar forumla to manipulate the password to another random string, one that is easy to generate from the original, but would take an extremely long time to generate the original from the hash.

Some examples of hashing formulas are [MD5](https://en.wikipedia.org/wiki/MD5) and [SHA256](https://en.wikipedia.org/wiki/SHA-2).

### Why isn't hashing enough?

Unfortunately, [rainbow tables](https://en.wikipedia.org/wiki/Rainbow_table) exist.

A rainbow table is a precomputed table of hashes for some set of passwords. Basically, since the hashing function is easy to compute one-way, people build huge tables of hashes wherein the plaintext is already known, so that attempting to crack hashes becomes a simple problem of looking up the hash in the table and it's corresponding value, instead of attempting to reverse the hash. Because of this, it is very easy to crack simple hashes by simply doing a lookup.

An example of a service that provides this is [Crackstation](https://crackstation.net/).

Since attackcs like rainbow tables exist, passwords need another layer of security.

## Adding salt

A salt refers to appending a string of text, unique to each user, to their password before hashing them. Since each user has a unique salt, this makes rainbow tables ineffective, as the majority of the precomputed hashes won't even contain the salt, so they wouldn't even matter anyway! In this way, the salt forces the attacker to recompute the rainbow table for each password in order to be able to effectively use it. This effectively converts the attack to brute force, as each hash must be recomputed for each possible password. Additionally, the computed rainbow table would only be useful for that specific user, as each user would have a different salt. It could take years before a password is cracked!

Thus, the way to store passwords properly is to use a salted hash - take the user's password, append some bytes to it, and hash the result. That hash is the user's password. When the user attempts to login again, perform the same procedure again. If the hash matches, then you know that the user is who he says he is, even if you don't actually know the original password that the user provided. 

One question that is commonly asked by developers is where to store the salt. The salt can be stored in plaintext, along with the user in the database. Since the goal of the salt is only to prevent precomputed rainbow tables from being used, it doesn't need to be encrypted in the database.

### If an attacker has gained access to the entire application, what does it matter if my passwords are stored in plaintext or not?

If an attacker has already gained access to the entire application, then he already has all the information hat he coculd possibly get. He would have access to all of your application's data, including data from your users or from any analytics software that you might be running. However, by adding salt and hashing your passwords, the attacker still doesn't know your customer's passwords and could take years to find out. Otherwise, since 59% of people use the same password across multiple sites<sup>1</sup>, the attacker could quickly try other websites such as banks to attempt to break into those accounts, which can yield great financial return.

Additionally, when a user signed up on your website and provided you a password, they implicitly trusted you to keep that information safe and secure for them, and in a sense you have a responsibility to keep their passwords secure as well. By doing proper password storage, if your servers ever get breached, you can assure your customers that their passwords is properly secured and maintain some of their trust in you.

## Word of warning

Reading this article doesn't make you an expert in writing your own password hashing code. It is far too easy to screw up and make a mistake. Instead, use one of the free libraries that provide a crypto function that has already been well-tested by the community. **Do not write your own crypto library.** For example, here are some libraries you can use instead depending on your language:

* If you're using PHP, the [password_hash](https://secure.php.net/manual/en/function.password-hash.php) function will do the job for you. 
* If you're using java, use [SecretKeyFactory](https://www.owasp.org/index.php/Hashing_Java) to apply hashes with SHA256.
* If you're using Python, use [hashlib](https://docs.python.org/3/library/hashlib.html) to salt and hash your password. [Example usage](https://stackoverflow.com/questions/9594125/salt-and-hash-a-password-in-python)
* If you're using npm, use [bcrypt](https://www.npmjs.com/package/bcrypt)

## Other resources

* [Secure Salted Password Hashing - How to do it properly](https://crackstation.net/hashing-security.htm) is an excellent resource that explains how to perform salted password hashing correctly, including links to other good libraries and what else can be done.
* [Awesome Cryptography](https://github.com/sobolevn/awesome-cryptography) is a curated list of resources - articles, blogs, books, libraries and more.

<sup>1</sup> https://securityboulevard.com/2018/05/59-of-people-use-the-same-password-everywhere-poll-finds/

</div>
