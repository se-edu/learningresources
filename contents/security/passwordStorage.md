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

## What is encryption?

Encryption refers to the process of changing a message such that it becomes essentially random text. The opposite process is called _decryption_, which converts the text back into the original message. In order to do this, it takes in an <trigger for="pop:encrypt-key">encryption key</trigger>, which is then re-used in the decryption process. 

Additionally, even a small change in the message will result in a completely different <trigger for="pop:ciphertext">ciphertext</trigger>. This means that even very similar passwords that vary only by capitalization or a letter would still result in different ciphertext, so attackers can't compare the hash to try and figure out which users have similar passwords.

<popover id="pop:ciphertext" title="_Ciphertext_ refers to encrypted information" placement="top">
  <div slot="content">
It is a cryptography term generally referring to data after encrypting it.
  </div>
</popover>

For example, the following python code encrypts and decrypts the message "EncryptionIsFun!" with <trigger for="pop:aes">AES</trigger>:
```python
from Crypto.Cipher import AES

AESCipher = AES.new("Here's a AES Key", AES.MODE_CBC, "Here's a AES IV.")
message = "EncryptionIsFun!"
ciphertext = AESCipher.encrypt(message)

AESCipher2 = AES.new("Here's a AES Key", AES.MODE_CBC, "Here's a AES IV.")
plaintext = AESCipher2.decrypt(ciphertext)

print(plaintext))
```

<popover id="pop:aes" title="_AES_ stands for Advanced Encryption Standard" placement="top">
  <div slot="content">
It is one of many possible encryption algorithms.
  </div>
</popover>

Encryption might seem like a good idea because it transfoms the data into essentially random text, which prevents all of the problems with storing the data directly in plaintext. However, because encryption is <tooltip content="If a function is reversible, and it converts from x to y, then it can also convert from y back to x"> reversible</tooltip>, it is always possible to regain the original password from the <trigger for="pop:ciphertext">ciphertext</trigger>. Since the data is encrypted, the <trigger for="pop:encrypt-key">encryption key</trigger> must also be stored somewhere. This means that if someone manages to hack into the application and read the encrypted passwords, it is also likely that they will be able to read the encryption key. With the encryption key, they will be able to decrypt all the passwords and read them anyway. This makes encryption unsuitable for password storage.

<popover id="pop:encrypt-key" title="All encryption algorithms require an _encryption key_" placement="top">
  <div slot="content">
The key is usually randomly generated text that is used to encrypt the original data. To then decrypt the data, the same key (and algorithm) must be used.
  </div>
</popover>

## What is hashing?

Hashing is a <trigger for="pop:oneway">one-way</trigger> function that transforms a set of data into another set of data. Unlike encryption, hashing works by re-iterating on the input multiple times, each time using values that are dependent on the previous state, then destroying the previous state. This makes it impossible to reverse as a computer would have to test all possible states on each iteration, which compounds into several billion possible states. 
<popover id="pop:oneway" title="" placement="top">
  <div slot="content">
    A one way function is a function that is easy to compute the result of, but whose results are difficult to reverse back to the original input.
  </div>
</popover>

<popover id="pop:hashing-algo" title="A _hashing algorithm_ is a specific type of operation that hashes the input" placement="top">
  <div slot="content">
Different types of hashing algorithms will result in different output.
  </div>
</popover>

Some examples of cryptographic <trigger for="pop:hashing-algo">hashing algorithms</trigger> are [MD5](https://www.quora.com/How-does-the-MD5-algorithm-work) and [SHA1](https://deadhacker.com/2006/02/21/sha-1-illustrated/). However, when you perform hashing for passwords, you should use password hashing algorithms (such as [Argon2](https://github.com/P-H-C/phc-winner-argon2), [SCrypt](https://passlib.readthedocs.io/en/stable/lib/passlib.hash.scrypt.html) and [bcrypt](https://security.stackexchange.com/questions/4781/do-any-security-experts-recommend-bcrypt-for-password-storage)). The main difference between these are that password hashing algorithms are designed to be slow to prevent <trigger for="pop:brute">brute force</trigger> attacks, unlike cryptographic hashing algorithms which are built for speed. Despite that, if you plan on learning more about hashing algorithms, we recommend starting with MD5 and SHA1, as they are easier algorithms to learn about.

For example, the following python code hashes the message "HashingIsFun!" with SHA1:
```python
import hashlib

sha1Hash = hashlib.sha1()
sha1Hash.update("HashingIsFun!".encode('utf-8'))
hashOutput = sha1Hash.hexdigest()
print(hashOutput)
```

### Why isn't hashing enough?

A [rainbow tables](https://en.wikipedia.org/wiki/Rainbow_table) is a precomputed table of <tooltip content="Hashes are the result of a hashing function">hashes</tooltip> for some set of passwords. Basically, people build huge tables of hashes wherein the plaintext is already known, so that attempting to crack hashes becomes a simple problem of looking up the hash in the table and its corresponding value, instead of attempting to reverse the hash. Through this method, it is very easy to crack simple hashes by simply doing a lookup.

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

### What if there is a server breach?

A common question asked by developers is how much all of these security measures actually matter. After all, if an attacker has gained access to the entire application, does it matter if passwords are stored in plaintext or not?

If an attacker has already gained access to the entire application, then he already has all the information that he could possibly want from the server. He would have access to all of the application's data, including data from users or from any analytics software that might be running. However, by adding salt and hashing passwords, the attacker still doesn't know customer's passwords and could take years to find out. Otherwise, since 59% of people use the same password across multiple sites <sup>[source](https://securityboulevard.com/2018/05/59-of-people-use-the-same-password-everywhere-poll-finds/)</sup>, the attacker could quickly try other websites such as banks to attempt to break into those accounts, which can potentially yield great returns in terms of information and/or money.

Additionally, when a user signed up on your website and provided you a password, they implicitly trusted you to keep that information safe and secure for them. In a sense, you do have a responsibility to keep their passwords secure. By doing proper password storage, if your servers ever get breached, you can assure your customers that their passwords are properly secured, and still maintain some of their trust in you.

## Getting started

<box type="danger">
It is far too easy to screw up and make a mistake. Instead, use one of the free libraries that provide a crypto function that has already been well-tested by the community. <b>Do not write your own crypto library.</b>
</box>

Here are some libraries you can use to implement password storage:
* PHP: [password_hash](https://secure.php.net/manual/en/function.password-hash.php)
* Java: [SecretKeyFactory](https://www.owasp.org/index.php/Hashing_Java) 
* Python: [PassLib](https://passlib.readthedocs.io/en/stable/narr/hash-tutorial.html)
* JavaScript: [bcrypt](https://www.npmjs.com/package/bcrypt)

## Other resources

* [Secure Salted Password Hashing - How to do it properly](https://crackstation.net/hashing-security.htm) is an excellent resource that explains how to perform salted password hashing correctly, including links to other good libraries and what else can be done.
* [Awesome Cryptography](https://github.com/sobolevn/awesome-cryptography) is a curated list of resources on cryptographic algorithms - articles, blogs, books, libraries and more.

</div>
