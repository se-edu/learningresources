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

**Author: [Jeremy Choo](https://github.com/ChooJeremy)** <br>
Reviewers: [Amrut Prabhu](https://github.com/amrut-prabhu), [Marvin Chin](https://github.com/marvinchin), [Tan Zhen Yong](https://github.com/Xenonym), [Wang Junming](https://github.com/junming403)

## Overview

Many software applications use a username and password combination as user account credentials for authentication. Obviously, it is not a good idea for the software to store these credentials as <trigger for="pop:plaintext">plain text</trigger> because if someone else were to gain access to them either lawfully (e.g., an employee who has access to the data) or unlawfully (e.g., someone hacking into the data storage), that person can use those credentials directly to impersonate the account owner. This article explains some techniques that can be used to store user credentials more securely:

- Encryption
- Hashing
- Salting

<popover id="pop:plaintext" title="_Plaintext_ refers to unencrypted information" placement="top">
  <div slot="content">
It is a cryptography term generally referring to text before encyption or after decrypting it. Another term for it is cleartext.
  </div>
</popover>

<popover id="pop:oneway" title="" placement="top">
<div slot="content">
	A one way function is a function that is easy to compute the result of, but whose results are difficult to reverse back to the original input.
</div>
</popover>

## What is encryption?

Encryption refers to the process of changing a message such that it becomes essentially random text. The opposite process is called decryption, which converts the text back into the original message. Encryption might seem like a good idea because it transfoms the data into essentially random bytes, which prevents all of the problems with storing the data directly in plaintext. However, because encryption is <tooltip content="If a function is reversible, and it converts from x to y, then it can also convert from y back to x"> reversible</tooltip>, it is always possible to regain the original password from the <trigger for="pop:ciphertext">ciphertext</trigger>.

<popover id="pop:ciphertext" title="_Ciphertext_ refers to encrypted information" placement="top">
  <div slot="content">
It is a cryptography term generally referring to data after encrypting it.
  </div>
</popover>

While encryption might seem suitable for storing passwords, it faces some severe problems. Since the data is encrypted, the encryption key must also be stored somewhere. This means that if someone manages to hack into the application and read the encrypted passwords, it is also likely that they will be able to read the encryptio key. With the encryption key, they will be able to decrypt all the passwords and read them anyway. This makes encryption unsuitable for password storage.

## What is hashing?

Hashing is the act of transforming a set of data into another set of data that is impossible to reverse. As an example, suppose you wanted to hash [the entire play of Romeo and Juliet](http://shakespeare.mit.edu/romeo_juliet/full.html). If you performed hashing using <tooltip content="A hashing function">SHA1</tooltip> on the entire contents of that file, excluding the header, you'll end up hashing the entire 6048 lines of that file into `7A52CDA9F04051B0D8263AA61A42531DD27FCE92`. 

Obviously, it is impossible to condense all 6048 lines of text into just those 40 characters. This means that is is impossible to recreate the entire play of Romeo and Juliet from just the hash result. Thus, we say that a hashing function is a one-way function, as it can only be performed in a one direction, and it would be impossible to reverse the process. 

Additionally, due to how <trigger for="pop:hashing-algo">hashing algorithms</trigger> work, even a small change in the file will result in a completely different hash. For example, if I were to change the first line from `ACT I` to `ACT IX`, it would change the hash to `D180AE51CEEFA8EF6D01F7FD54528AC21659433E`, completely different from the previous hash. This means that even very similar passwords that vary only by capitalization or a letter would still result in different <tooltip content="Hashes are the result of a hashing function">hashes</tooltip>, so attackers can't compare the hash to try and figure out which users have similar passwords.

<popover id="pop:hashing-algo" title="A _hashing algorithm_ is a specific type of operation that hashes the input" placement="top">
  <div slot="content">
Different types of hashing algorithms will result in different output.
  </div>
</popover>

Some examples of cryptographic hashing algorithms are [MD5](https://www.quora.com/How-does-the-MD5-algorithm-work) and [SHA1](https://deadhacker.com/2006/02/21/sha-1-illustrated/). However, when you perform hashing for passwords, you should use password hashing algorithms (such as [Argon2](https://github.com/P-H-C/phc-winner-argon2), [SCrypt](https://passlib.readthedocs.io/en/stable/lib/passlib.hash.scrypt.html) and [bcrypt](https://security.stackexchange.com/questions/4781/do-any-security-experts-recommend-bcrypt-for-password-storage)). The main difference between these password hashing algorithms are designed to be slow to prevent <trigger for="pop:brute">brute force</trigger> attacks, unlike cryptographic hashing algorithms which are built for speed. Despite that, if you plan on learning more about hashing algorithms, we recommend starting with MD5 and SHA1, as they are easier algorithms to learn about.

### Why isn't hashing enough?

Because [rainbow tables](https://en.wikipedia.org/wiki/Rainbow_table) exist, unfortunately.

A rainbow table is a precomputed table of hashes for some set of passwords. Basically, since the hashing function is easy to compute one-way, people build huge tables of hashes wherein the plaintext is already known, so that attempting to crack hashes becomes a simple problem of looking up the hash in the table and its corresponding value, instead of attempting to reverse the hash. Because of this, it is very easy to crack simple hashes by simply doing a lookup.

An example of a service that provides this is [Crackstation](https://crackstation.net/).

Since attacks like rainbow tables exist, passwords need another layer of security.

## Adding salt

A salt refers to appending a string of text, unique to each user, to their password before hashing them. Since each user has a unique salt, this makes rainbow tables ineffective, as the majority of the precomputed hashes won't even contain the salt, so they wouldn't even matter anyway! In this way, the salt forces the attacker to recompute the rainbow table for each password in order to be able to effectively use it. This effectively converts the attack to <trigger for="pop:brute">brute force</trigger>, as each hash must be recomputed for each possible password. Additionally, the computed rainbow table would only be useful for that specific user, as each user would have a different salt. It could take years before a password is cracked!

<popover id="pop:brute" title="A brute force attack is an attack where all possible combinations are tested to see if they work." placement="top">
  <div slot="content">
 A brute force attack usually takes very long to carry out. A cryptographic hashing algorithm helps 
  </div>
</popover>

Note that the salt should be randomly generated, as opposed to choosing a static value that is different for every user. For example, if an application used the username of the user as the salt, then attackers can pre-generate rainbow tables for common usernames, causing users with common users to be vulnerable to a rainbow table attack, even with salt applied to their passwords.

Thus, the way to store passwords properly is to use a salted hash - take the user's password, append some bytes to it, and hash the result. That hash is the user's hashed password. When the user attempts to log in again, perform the same procedure again. If the hash matches, then you know that the user is who he says he is, even if you don't actually know the original password that the user provided. 

One question that is commonly asked by developers is where to store the salt. The salt can be stored in plaintext, along with the user in the database. Since the goal of the salt is only to prevent precomputed rainbow tables from being used, it doesn't need to be encrypted in the database.

Many good password hashing algorithms today have built-in salts, such as [Argon2](https://github.com/P-H-C/phc-winner-argon2), [SCrypt](https://passlib.readthedocs.io/en/stable/lib/passlib.hash.scrypt.html) and [bcrypt](https://security.stackexchange.com/questions/4781/do-any-security-experts-recommend-bcrypt-for-password-storage). A good password hashing algorithm or library will salt automatically.

### If an attacker has gained access to the entire application, what does it matter if passwords are stored in plaintext or not?

If an attacker has already gained access to the entire application, then he already has all the information that he could possibly want from the server. He would have access to all of the application's data, including data from users or from any analytics software that might be running. However, by adding salt and hashing passwords, the attacker still doesn't know customer's passwords and could take years to find out. Otherwise, since 59% of people use the same password across multiple sites <sup>[source](https://securityboulevard.com/2018/05/59-of-people-use-the-same-password-everywhere-poll-finds/)</sup>, the attacker could quickly try other websites such as banks to attempt to break into those accounts, which can potentially yield great returns in terms of information and/or money.

Additionally, when a user signed up on your website and provided you a password, they implicitly trusted you to keep that information safe and secure for them. In a sense, you do have a responsibility to keep their passwords secure. By doing proper password storage, if your servers ever get breached, you can assure your customers that their passwords are properly secured, and still maintain some of their trust in you.

## Getting started

<p style="color: #721c24; background-color: #f8d7da; border-color: #f5c6cb; padding: .75rem 1.25rem; border: 1px solid transparent; border-radius: .25rem;">
Reading this article doesn't make you an expert in writing your own password hashing code. It is far too easy to screw up and make a mistake. Instead, use one of the free libraries that provide a crypto function that has already been well-tested by the community. <b>Do not write your own crypto library.</b>
</p>

Here are some libraries you can use to implement password storage, depending on the language:
* If you're using PHP, the [password_hash](https://secure.php.net/manual/en/function.password-hash.php) function will do the job for you. 
* If you're using Java, use [SecretKeyFactory](https://www.owasp.org/index.php/Hashing_Java) to apply hashes with SHA256.
* If you're using Python, use [PassLib](https://passlib.readthedocs.io/en/stable/narr/hash-tutorial.html).
* If you're using JavaScript, use [bcrypt](https://www.npmjs.com/package/bcrypt).

## Other resources

* [Secure Salted Password Hashing - How to do it properly](https://crackstation.net/hashing-security.htm) is an excellent resource that explains how to perform salted password hashing correctly, including links to other good libraries and what else can be done.
* [Awesome Cryptography](https://github.com/sobolevn/awesome-cryptography) is a curated list of resources on cryptographic algorithms - articles, blogs, books, libraries and more.

</div>
