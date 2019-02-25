<frontmatter>
  title: Accessibility
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Accessibility

Author(s): [Monika Manuela Hengki](https://github.com/monmanuela)

## What is accessibility?

> Accessibility is the practice of making your websites usable by as many people as possible — we traditionally think of this as being about people with disabilities, but really it also benefits other groups such as those using mobile devices, or those with slow network connections.
>
> Taken from [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/What_is_accessibility)

Accessibility enables users, regardless or their ability or circumstances, to perceive, interact and navigate through a website without barriers. Just like how buildings have wheelchair ramps or elevators to help wheelchair-bound people enter a building, a website can have some nifty tricks to help users with disabilities navigate through it.

## Why is accessibility important?

##### For the developer
1. **Accessibility improves everyone's UX**. Accessibility practices are good design practices in general, so improving accessibility also makes your website more usable by other groups such as mobile phone users, those on a low network speed, etc.
2. **Accessibility is a right**. Caring about accessibility demonstrates good ethics/morals, which improves your public image.
3. **Accessibility improves SEO**. Semantic HTML (which improves accessibility) also improves SEO, making your website more findable and marketable to new users.
4. **Accessibility helps you increase your user base**. According to the [World Health Organization world report on disability](https://www.who.int/disabilities/world_report/2011/report/en/), about 15% of the world's population live with some form of disability. That makes up more than 1 billion people. It is a significant population of users you can reach just by improving accessibility.
5. **Accessibility is part of the law in some places**. In [some countries](https://www.w3.org/WAI/policies/), abiding by accessibility guidelines is mandatory. Be careful not to break the law!

##### For the user
Accessibility is important for a lot of people with disabilities to access the Web. There are diverse kinds of disabilities, including:
   * Visual (partial blindness, full blindness, color blindness, cataract, glaucoma, etc.)
   * Auditory (hard of hearing, deafness, etc.)
   * Cognitive (ADHD, autism spectrum, etc.)
   * Mobility (quadriplegia, muscular dystrophy, etc.)

At the same time, accessibility also benefits people _without_ disabilities, for example:
   * people using mobile phones, smart watches, smart TVs, and other devices with small screens, different input modes, etc.
   * older people with changing abilities due to ageing
   * people with “temporary disabilities” such as a broken arm or lost glasses
   * people with “situational limitations” such as in bright sunlight or in an environment where they cannot listen to audio
   * people using a slow Internet connection, or who have limited or expensive bandwidth

## How can I start improving accessibility?

There are many ways to improve accessibility of your website. Below are some tips, divided into the type of disabilities they address.

### Visual
Users with visual impairments rely on assistive technologies such as a magnifier or a screen reader.

  * Use more visual indicators to convey a message.
  
  Let us look at Facebook sign up page. Suppose I want to sign up for a new account, but I have not put in all the necessary information. So, it is supposed to tell me that my attempt to sign up failed.

  This page below uses a red color border around the text box to show that the information needed is missing. As red usually signifies failure, this seems enough.
  ![Facebook sign up page](https://i.imgur.com/HJPD8Fu.png)

  However, to someone with color blindness, this is how the page looks like
  ![Facebook sign up page to someone with achromatopsia](https://i.imgur.com/lsI5YI8.png)
  _[Achromatopsia](https://en.wikipedia.org/wiki/Achromatopsia)_

  ![Facebook sign up page to someone with deuteranopia](https://i.imgur.com/uC9kcTb.png)
  _[Deuteranopia](https://en.wikipedia.org/wiki/Deuteranopia)_

  It is unclear to color-blind users that the sign up has failed. To avoid confusion, we should not rely on colours alone to convey a message. Instead, we should use more visual indicators such as icons or an explanation box.
  ![Sign up page with more visual indicators](https://i.imgur.com/jA0cfKd.png)

  * Maintain good contrast
  
  Look at this page taken from Tech Crunch.
  ![Tech Crunch page](https://i.imgur.com/sgBuXNL.png)

  To users with good vision, the design may look minimalist and clean. However, to someone suffering from cataract, the page looks like this.

  ![Blurry Tech Crunch page](https://i.imgur.com/QapHc6t.png)
  It gets difficult to read the news snippets because of the poor color contrast (grey on white). On the other hand, the black colored texts are still legible. Thus, we should maintain good contrast ratio in our websites for ease of reading.

  * Use `alt` attribute for images
  
  The `alt` attribute provides alternative information for an image that can be read out by a screen reader to describe an image.

  ![Use of alt tag](https://i.imgur.com/Jted6hW.png)

  
### Auditory
Hearing-impaired users do use assistive technologies such as a hearing aid or a cochlear implant, but these are not specific for accessing websites.

For users with hearing impairment, we should provide text alternatives to audio content, such as:
  * Text transcripts
  * Captions

  
### Cognitive
Cognitive disabilities are diverse. They can refer to mental illnesses to learning difficulties, difficulties in comprehension and concentration, etc. Some examples include ADHD (attention deficit hyperactivity disorder), and autism.

Such disabilities might affect website usage is due to difficulty in understanding how to complete a task, remembering how to do something that was previously accomplished, or increased frustration at confusing workflows or inconsistent layouts/navigation/other page features.

Unlike other web accessibility issues, there is no quick fixes to issues arising from cognitive disabilities. The rule of thumb you can follow is to always design your websites to be as logical, consistent, and usable as possible. Here is a checklist that you can follow:

* Pages are consistent — navigation, header, footer, and main content are always in the same places.
* Tools are well-designed and easy to use.
* Multi-stage processes are broken down into logical steps, with regular reminders of how far through the process you are, and how long you've got left to complete the process, if appropriate.
* Workflows are logical, simple, and require as few interactions as possible to complete. For example, registering and signing in to a website is often unneccessarily complex.
* Pages are not overly long or dense in terms of the amount of information presented at once.
* The language used in your pages is as plain and easy to follow as possible, and not full of unneccessary jargon and slang.
* Important points and content are highlighted in some way.
* User errors are clearly highlighted, with help messages to suggest solutions.

_Taken from [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/What_is_accessibility#People_with_cognitive_impairments)_

WebAIM's [Cognitive](http://webaim.org/articles/cognitive/) page provides a useful expansion of these ideas, and is certainly worth reading.

### Mobility
Mobility impairments include paralysis, physical weakness or loss of control in limbs. This can cause users to find it difficult or impossible to use a mouse as the main form of interaction with websites.

Assistive devices include a [switch access](https://en.wikipedia.org/wiki/Switch_access), or a [head pointer](https://www.performancehealth.com/baseball-cap-head-pointer). Users may also use a keyboard instead of a mouse to interact with the website.

The key to improve accessibility for mobility is to make the website keyboard accessible.

WebAIM's article on [keyboard accessibility](https://webaim.org/techniques/keyboard/) provides an amazing tutorial for this.

  
## Ending Notes
Ultimately, the most important thing you need to start designing accessible websites is empathy for your users. Each user is unique, and each user has different needs. As a developer, you need to step in the users' shoes, understand their pain points, and then develop solutions for them so that everyone can navigate through your website freely.
  

## Useful Resources
You are ready for your accessibility journey! Here are some resources to help you get started:
* [**The A11Y Project**](https://a11yproject.com/resources). The Accessibility Project is an open-source resource library on accessibility. It seeks to make it easier for developers to implement accessible websites by providing tips, tutorials and a widget & pattern library.
* [**Web Content Accessibility Guidelines (WCAG)**](https://www.w3.org/WAI/standards-guidelines/wcag/). WCAG provides a single shared standard for web content accessibility that meets the needs of individuals, organizations, and governments internationally.
* [**Web Accessibility in Mind (WebAIM)**](https://webaim.org/articles). WebAIM contains well-written articles on specific topics on accessibility issues and how to tackle them.

</div>
