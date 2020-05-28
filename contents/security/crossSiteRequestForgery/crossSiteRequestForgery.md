<frontmatter>
  title: Cross Site Request Forgery (CSRF)
  pageNav: 3
</frontmatter>

<div class="website-content">

# Cross Site Request Forgery (CSRF)

Author: [Tran Tien Dat](https://github.com/tran-tien-dat)

<box id="article-toc">

* [Flow of a CSRF attack‎](#flow-of-a-csrf-attack)
  * [Step 1: The Victim Logs in to a Vulnerable Web Service‎](#step-1-the-victim-logs-in-to-a-vulnerable-web-service)
  * [Step 2: The Victim Visits an Untrusted Website Controlled by the Attacker‎](#step-2-the-victim-visits-an-untrusted-website-controlled-by-the-attacker)
  * [Step 3: The Untrusted Website Makes Requests to the Vulnerable Web Service on the Victim's Behalf‎](#step-3-the-untrusted-website-makes-requests-to-the-vulnerable-web-service-on-the-victim-s-behalf)
* [Conditions for a Successful CSRF Attack‎](#conditions-for-a-successful-csrf-attack)
* [Defense Against CSRF Attacks‎](#defense-against-csrf-attacks)
* [References‎](#references)
</box>

Cross-Site Request Forgery (CSRF) is a dangerous type of attack that has affected major sites like [Gmail](http://archive.oreilly.com/pub/post/gmail_exploit_contact_list_hij.html) and [Netflix](https://blog.jeremiahgrossman.com/2006/10/more-on-netflixs-csrf-advisory.html) in the past. This article attempts to give an easy-to-digest introduction to the attack and how to protect your website from it.

## Flow of a CSRF Attack

A CSRF attack tricks the victim to perform actions that they do not intend to do on a web application in which they're currently authenticated. It generally consists of 3 steps:
1. The victim logs in to a vulnerable web service
2. The victim visits an untrusted website controlled by the attacker
3. The untrusted website makes requests to the vulnerable web service on the victim's behalf

These steps are best explained through an example. Suppose that Alice is a customer of the banking website `www.example-bank.com`. Alice wants to transfer money to her friend Bob (because she does not like the fact that he keeps paying for her meals when they go out together). 

### Step 1: The Victim Logs in to a Vulnerable Web Service

Alice first logs in to the website at `www.example-bank.com/login`, providing her username and password. She then clicks on a hyperlink on the site to go to the URL `www.example-bank.com/transfer` to perform her transaction. Note that Alice does not have to enter her username and password again, but the bank still knows that the request is made by her. This is done through a mechanism of the HTTP protocol called a [HTTP Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies). A *cookie* is a small piece of information that is sent along with all HTTP requests to a particular website. In our case, after receiving Alice's credentials for logging in, the bank instructs Alice's browser to store a *cookie* `auth=1abcd2ek3292fsa390sdf` and **send this cookie together with every subsequent request** to `www.example-bank.com`. Thus, once the web server sees this cookie in the request, it knows that this is an authenticated request from Alice. Many websites use this cookie mechanism so that users only have to log in once.

At `www.example-bank.com/transfer`, Alice fills out the following HTML form to transfer money to Bob:

```HTML
<form action="/transfer" method="POST">
<input type="text" name="receiver-name" value="Bob"/>
<input type="number" name="receiver-account-no" value="123456"/>
<input type="number" name="amount" value="100"/>
<input type="submit"/>
</form>
```

As Alice clicks the submit button, the browser will send the following request to `www.example-bank.com`:

```
POST https://www.example-bank.com/transfer HTTP/1.1
Cookie: auth=1abcd2ek3292fsa390sdf

receiver-name=Bob&receiver-account-no=123456&amount=100
```

The bank's server verifies that the cookie `auth=1abcd2ek3292fsa390sdf` is valid and is associated with Alice. It then proceeds to transfer $100 from Alice's account to account number 123456, which belongs to Bob.

### Step 2: The Victim Visits an Untrusted Website Controlled by the Attacker

Alice then receives a chat message from her not-so-trustworthy friend, Eve:

```
Eve: OMG Alice! This site is selling the shoes you have always
     wanted for half the price: www.i-am-not-evil.com/shoes
```

Excited by this piece of news, Alice clicks on the hyperlink to check out `www.i-am-not-evil.com`.

### Step 3: The Untrusted Website Makes Requests to the Vulnerable Web Service on the Victim's Behalf

While browsing `www.i-am-not-evil.com`, Alice clicks on the `View more pictures` button on the website to see more pictures of her favorite pair of shoes. Unbeknownst to her, that button has the following HTML code:

```HTML
<form action="https://www.example-bank.com/transfer" method="POST">
<input type="text" name="receiver-name" value="Eve"/>
<input type="number" name="receiver-account-no" value="987654"/>
<input type="number" name="amount" value="100000"/>
<input type="submit" value="View more pictures"/>
</form>
```

So, when she clicks that button, the browser actually sends the following request to `https://www.example-bank.com`:

```
POST https://www.example-bank.com/transfer HTTP/1.1
Cookie: auth=1abcd2ek3292fsa390sdf

receiver-name=Eve&receiver-account-no=987654&amount=100000
```

Since Alice is still logged in to `www.example-bank.com`, the browser automatically attaches the cookie `auth=1abcd2ek3292fsa390sdf` to any requests made to `www.example-bank.com`. Hence, the bank's server considers this as an authenticated request from Alice and proceeds to transfer $100,000 from Alice's account to account number 987654, which belongs to the attacker!

## Conditions for a Successful CSRF Attack

From the example, we can see that a CSRF attack works by forging a valid request which **inherits the identity and privileges of the victim** on the vulnerable web server. Such inheritance of privileges is possible because the browser attaches any credentials associated with a website to all requests made to the site. Therefore, if the user is currently authenticated to the site, the site will have no way to distinguish between the forged request sent by the victim and a legitimate request sent by the victim. In short, there are 3 conditions necessary for the attack:

1. The target website is vulnerable to a CSRF attack (i.e. it cannot distinguish between forged and legitimate requests).
2. The victim is authenticated to the target website.
3. The victim is tricked to visit an untrusted website by social engineering.

*Note: [Social engineering](https://en.wikipedia.org/wiki/Social_engineering_%28security%29) refers to psychological manipulation of people into performing actions. In our example, Eve shares a link with a description that matches Alice's interest, which increases the chance that Alice will follow the link. Thus, this action is a form of social engineering.*

## Defense Against CSRF Attacks

Both users and websites can take actions to defend themselves against CSRF attacks. As the user, one can reduce the likelihood of being attacked by logging out of sensitive services like banking immediately after carrying out transactions. This will negate condition number 2. Condition number 3 can be mitigated if the user is extremely careful and never visits websites they do not know. However, users generally do not follow this and just click on interesting links they encounter on social media. Hence, this threat of social engineering could never be fully eliminated.

As for the web server, it can eliminate condition number 1 by adding a secret token as a hidden field to all forms that it serves. This token should be random and cannot be guessed by the attacker. Back to our example, the bank website should use a form like the following:

```HTML
<form action="/transfer" method="POST">
<input type="hidden" name="csrf-token" value="abcd23ksk3l2faad2kdkl"/>
<input type="text" name="receiver-name" value="Bob"/>
<input type="number" name="receiver-account-no" value="123456"/>
<input type="number" name="amount" value="100"/>
<input type="submit"/>
</form>
```

So a valid request by Alice made from the bank's own website will also carry this secret token `csrf-token=abcd23ksk3l2faad2kdkl`. Since the token is added by the bank's website on its webpages, forged requests from other websites will not know the token. Thus, when the bank's server receives a request, it can distinguish between a legitimate and a forged request by checking that the CSRF token is present and valid.

## References

- https://owasp.org/www-community/attacks/csrf (A more technical description of the attack)
- https://owasp.org/www-project-cheat-sheets/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html (In-depth discussion of the various defense approaches, including those that do not work)
- https://www.cgisecurity.com/csrf-faq.html (Short FAQs about CSRF)
- https://docs.djangoproject.com/en/2.0/ref/csrf/ (CSRF Protection in Django)

</div>
