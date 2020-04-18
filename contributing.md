<frontmatter>
  title: "Contributing to this Project"
  header: pagetop.md
  footer: footer.md
  head: head.md
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Contributing to this Project

We welcome contributions to this project.

## How to Contribute

You can report errors, send suggestions, ask questions. <trigger trigger="click" for="modal:contributing-contactInfo">Here</trigger> is how you can contact us.

You can also add new topics or update existing content by following these steps:
* [Fork this repo](https://github.com/se-edu/learningresources/fork) and clone it to your Computer.
* Install [MarkBind](https://markbind.github.io) in your Computer.
* Post in [our issue tracker](https://github.com/nus-oss/learningresources/issues) an outline of your proposed contribution. Wait for our response.
* After receiving the go-ahead from us, send your proposed contribution as a PR.

<modal large header="How to contact us" id="modal:contributing-contactInfo">
  <include src="about.md#contact-info" />
</modal>

## Guidelines for Writing Content

* **Do include a table of contents.** A table of contents tells the reader what to expect, such as how long the article is and what content is covered. It also serves as a reference that makes it easier to jump around from section to section. In our case, we only show the table of contents for mobile users and for printing as Markbind already provides a navigation panel on the right side of every article. Feel free to refer to this [pull request](https://github.com/se-edu/learningresources/pull/185) to see how to implement this behavior.
* **Do use Title Case for headings.** Using title case for headings gives articles a more polished, professional look that is more pleasing to the eye. If you are unfamiliar with the rules of title casing, you may refer to the following [website](https://apastyle.apa.org/style-grammar-guidelines/capitalization/title-case) or use online title case converters such as the one found [here](https://titlecase.com/).
* **It is not a normal textbook. It’s a collection of articles**, each containing a **_curated_** list of **_tried-and-tested_** learning resources, **_organized_** in a meaningful sequence, with **_commentary_**.
  * The article can be on any technical topic useful for SE students.
  * Don’t try to follow the structure of normal textbooks.
  * There’s no need to use a very formal tone that you see in some text books. Instead, prefer an informal tone you would use in a blog post or an stackoverflow question.
  * The article should point to the _best_ (in your opinion) external learning resources for the topic. That means it should not include resources that you did not try yourself.
* ****The suggested structure**** for an Article
  * **What is X?**: Explain how X fits into the big picture of SE. Describe it relative to topics the reader is likely to know.
  * **Why X?**: Motivate the reader to learn about X. Describe benefits of X to make the reader interested in X. Try to give a balanced view of by mentioning also WHY NOT X i.e., mention both advantages and disadvantages.
  * **How does X work?** How is X being used? This is a simple high-level overview of the tool to give the reader some concrete sense of X (as opposed to limiting to an entirely abstract description). It's useful to give concrete examples such as code examples. ==Do not try to 'teach' how to use the X== (assuming your in the style of a tutorial. If the tool is worth learning, there must be good tutorials about it already.
  * **How to Get Started with X?**: Provide a learning path for the reader. Try to give one good learning path rather than many random resources.
  * **Where to Go from Here?**: Give more resources. Instead of listing a lot of links, provide a brief summary of what value each resource can provide the reader.
* **Do not assume a lot of prior knowledge on the part of the reader.** The less prior knowledge you assume, the wider your reader base becomes. The article is aimed at typical SE students, but not necessarily SE students in your school. Just because it is covered in a core module in your school does not mean it will be known to every reader.
* **Do not re-invent the wheel by writing a lot of original content.** Instead, give a brief high-level view and point to other existing resources that you recommend the reader to use.
  original source.
  * Note that ==the reader should still get a complete and useful picture== even if she does not refer any of the given links. i.e., make the content self-contained and grounded/concrete instead of too abstract to be of much use. To that end, you may repeat/adapt content snippets from other resources (instead of simply giving a link) with proper attribution.
  * If you reuse assets from other sources %%(e.g., diagrams)%%, remember to ==cite the original source==.
* **Stay an _independent_ observer**. When writing about a tool/technique that has competing alternatives, the article will have more credibility if you write it from the point of view of an independent observer. For example, avoid unsubstantiated marketing claims e.g., _X is the best tool for doing Y_. Instead, you can cite quotes by other credible sources or from the tool/technique itself. See the two examples below:
  > Tool X claims to have the fastest performance [[source]()].
  
  > [Tool X's website]() has the following to say about its performance:
  >> Tool X has the best performance of its class. ...
* **Prefer visuals rather than long paragraphs.**
* **Some of the existing articles might not follow the above guidelines** (the guidelines emerged over time). Feel free to revise existing content to fit the guidelines.

<box type="tip">

A fairly decent example of applying the above guidelines can be found in the article [_Introduction to Go_]({{ baseUrl }}/contents/go/Go.html).
</box>



</div>
