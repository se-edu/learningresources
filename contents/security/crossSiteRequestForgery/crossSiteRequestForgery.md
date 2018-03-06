# Cross Site Request Forgery (CSRF)

Author: [Tran Tien Dat](https://github.com/tran-tien-dat)

CSRF is a dangerous type of attack that has affected major sites like [Gmail](http://archive.oreilly.com/pub/post/gmail_exploit_contact_list_hij.html) or [Netflix](http://blog.jeremiahgrossman.com/2006/10/more-on-netflixs-csrf-advisory.html) in the past. This article attempts to give an easy-to-digest introduction to the attack and how to protect your website from it.

## Example

Cross-Site Request Forgery (CSRF) is an attack that tricks the victim to perform unwanted actions on a web application in which they're currently authenticated. 

To understand how the attack works, let's first take a look at an example. Suppose that Alice is a customer of the banking website `www.example-bank.com`. Alice wants to transfer money to her friend Bob (because she does not like the fact that he keeps paying for her meals when they go out together). 

Alice first logs in to the website at `www.example-bank.com/login`, providing her username and password. She then clicks on a hyperlink on the site to go to the URL `www.example-bank.com/transfer` to perform her transaction. Note that Alice does not have to enter her username and password again, but the bank still knows that the request is made by her. This is because after receiving Alice's credentials for logging in, the bank instructs Alice's browser to store a *cookie* `auth=1abcd2ek3292fsa390sdf` and **send this cookie together with every subsequent requests** to `www.example-bank.com`. Thus, once the web server sees this cookie in the request, it knows that this is an authenticated request from Alice.

At `www.example-bank.com/transfer`, Alice fills out the following HTML form to transfer money to Bob:

```
<form action="/transfer" method="POST">
<input type="number" name="account-no" value="123456"/>
<input type="number" name="amount" value="100"/>
<input type="submit"/>
</form>
```

As Alice clicks the submit button, the browser will sending the following request to `www.example-bank.com`:

```
POST http://www.example-bank.com/transfer HTTP/1.1
Cookie: auth=1abcd2ek3292fsa390sdf

account-no=123456&amount=100
```

The bank's server verifies that the cookie `auth=1abcd2ek3292fsa390sdf` is valid and is associated with Alice. It then proceeds to transfer $100 from Alice's account to account number 123456, which belongs to Bob.

Alice then receives a chat message from her not-so-trustworthy friend, Eve:

```
Eve: OMG Alice! This site is selling the shoes you have always wanted for half the price www.i-am-not-evil.com/shoes
```

Excited by this piece of news, Alice clicks on the hyperlink to checkout `www.i-am-not-evil.com`. She then clicks on the `View more pictures` button on the website to see more pictures of her favorite pair of shoes. Unbeknownst to her, that button has the following HTML code:

```
<form action="http://www.example-bank.com/transfer" method="POST">
<input type="hidden" name="account-no" value="987654"/>
<input type="hidden" name="amount" value="100000"/>
<input type="submit" value="View more pictures"/>
</form>
```

So, when she clicks that button, the browser actually sends the following request to `http://www.example-bank.com`:

```
POST http://www.example-bank.com/transfer HTTP/1.1
Cookie: auth=1abcd2ek3292fsa390sdf

account-no=987654&amount=100000
```

Since Alice is still logged in to `www.example-bank.com`, any request made to `www.example-bank.com` is automatically attached the cookie `auth=1abcd2ek3292fsa390sdf`. Hence, the bank's server considers this as an authenticated request from Alice and proceeds to transfer $100,000 from Alice's account to account number 987654, which belongs to the attacker! Indeed, `www.i-am-not-evil.com` could have performed the attack by using Javascript to submit the form, which would eliminate the need for Alice to click on anything, so she will not even be aware of it.

## Conditions for a successful CSRF attack

From the example, we can see that a CSRF attack works by forging a valid request which **inherits the identity and privileges of the victim** to the vulnerable web server. Such inheritance of privileges is possible because the browser attaches any credentials associated with a website to all requests made to the site. Therefore, if the user is currently authenticated to the site, the site will have no way to distinguish between the forged request sent by the victim and a legitimate request sent by the victim. In short, there are 2 conditions necessary for the attack:

1. The target website is vulnerable to a CSRF attack (i.e. it cannot distinguish between forged and legitimate requests).
2. The victim is authenticated to the target website.
3. The victim is tricked to visit an untrusted website by social engineering.

## Defense against CSRF attacks

Defense against CSRF can come from both the website and the user. As the user, one can reduce the likelihood of being attacked by logging out of sensitive services like banking immediately after carrying out transactions. This will negate condition number 2.

For the web server, it can eliminate condition number 1 by adding a secret token as a hidden field to all forms that it serves. This token should be random and cannot be guessed by the attacker. Back to our example, the bank website should server a form like the following:

```
<form action="/transfer" method="POST">
<input type="hidden" name="csrf-token" value="abcd23ksk3l2faad2kdkl"/>
<input type="number" name="account-no" value="123456"/>
<input type="number" name="amount" value="100"/>
<input type="submit"/>
</form>
```

So a valid request by Alice made from the bank's own website will also carry this secret token. Forged requests from other websites will not know the token. Thus, when the bank's server receives a request, it can distinguish between a legitimate and a forged request by checking that the CSRF token is present and valid.

## References

- https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF) (A more technical description of the attack)
- https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet (In-depth discussion of the various defense approaches, including those that do not work)
- http://www.cgisecurity.com/csrf-faq.html (Short FAQs about CSRF)
- https://docs.djangoproject.com/en/2.0/ref/csrf/ (CSRF Protection in Django)



