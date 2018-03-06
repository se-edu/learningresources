# Cross Site Request Forgery (CSRF)

Author: [Tran Tien Dat](https://github.com/tran-tien-dat)

## Example of a CSRF attack

Cross-Site Request Forgery (CSRF) is an attack that tricks the victim to perform unwanted actions on a web application in which they're currently authenticated. It works by forging a valid request which **inherits the identity and privileges of the victim** to the vulnerable web server. Such inheritance of privileges is possible because the browser attaches any credentials associated with a website to all requests made to the site. Therefore, if the user is currently authenticated to the site, the site will have no way to distinguish between the forged request sent by the victim and a legitimate request sent by the victim.

To better understand how the attack works, let's take a look at an example. Suppose that Alice is a customer of the banking website `www.example-bank.com`. Alice wants to transfer money to her friend Bob (because she does not like the fact that he keeps paying for her meals when they go out together). 

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

## Conditions for a successful CSRF attack

## Defenses against CSRF attacks
