<frontmatter>
  title: UI/UX
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Introduction to UI/UX

Authors: [Ang Shi Ya](https://github.com/AngShiYa), [Tan Jun Kiat](https://github.com/junkiattan)

<box id="article-toc">

* [Overview](#overview)
* [User Interface vs User Experience](#user-interface-vs-user-experience)
* [UI Design](#ui-design)
* [UX Design](#ux-design)
* [Which is more important?](#which-is-more-important)
* [The Process of UX Design](#the-process-of-ux-design)
  * [Preliminary Research](#preliminary-research)
  * [Prototyping](#prototyping)
  * [User Testing](#user-testing)
  * [Maintenance](#maintenance)
  * [Ending Notes](#ending-notes)
</box>

## Overview

User Interface (UI) and User Experience (UX) are terms often used interchangeably. In fact, these two terms are quickly becoming dangerous buzzwords that are used inaccurately, creating massive confusion in the industry. A simple Google search will show that UI Designer and UX Designer jobs are listed under similar titles and descriptions. 

In addition to helping us identify and apply for the right role, understanding the key differences between UI and UX helps us in using the right concept in our own applications. This document will address the difference between UI and UX as well as some of the techniques used in UI Design and UX Design.

## User Interface vs User Experience

UI is about the design of the buttons, the layout of the website and the responsiveness of the application. It encompasses the presentation and interactivity of the application. On the other hand, UX is concern with the ease of use and the pleasure provided through using the application. In short, UI is about the look and feel while UX is about customer satisfaction.

Due to the overlapping nature of UI and UX, you might still have some questions such as *'Doesn't improving the UI also improve the UX?'* Indeed, it does. In fact, UI **can** be part of UX.

> Note that **can** is used because UI is no longer a necessity to UX due to the emergence of [zero UI](https://blog.careerfoundry.com/ui-design/what-is-zero-ui), which won't be covered here.

To gain a better understanding, let's look at real life examples of good UI but bad UX. A prime example would be the [Mystery Meat Navigation](https://www.techinasia.com/talk/material-design-mystery-meat-navigation-problem) problem. It refers to buttons and links that do not explain themselves; users have to click on them to find out where it leads. In essence, the pursuit of clean and neat UI has resulted in omission of details that provide clarity for the user. This results in bad UX because people do not like to puzzle over how things work. A good UX should never make the user think.

If you would like to read more about the differences between UI and UX, here are a few articles:

* [The Difference Between UX and UI Design-A Layman’s Guide](https://blog.careerfoundry.com/ui-design/the-difference-between-ux-and-ui-design-a-laymans-guide/) - provides breakdown of the disciplines involved.
* [What’s the difference between UI and UX?](https://www.usertesting.com/blog/2016/04/27/ui-vs-ux/) - provides opinions from UX experts.
* [The superpowers of UX & UI designers](https://www.ymedialabs.com/ux-vs-ui/) - provides illustrations to highlight the differences.

## UI Design

Flat and minimalist design are technically easier to create than realistic design. Thanks to the rise of flat and minimalist design you don't have to be a photoshop expert to get started on UI design. 

A good way to start is by observing existing designs. Use existing UI Designs as references and try to apply the design patterns in your own application. Using [UI design guidelines](https://www.goodui.org/) can help to highlight and avoid common mistakes. Online lessons like [Hack Design](https://hackdesign.org/lessons#graphic-design-principles) also provide a variety of materials covering the basics of UI design. The best thing is that they are free!

Here is a non-exhaustive list of UI design concepts you can choose to dive into if you are interested:
* Color Theory - Take a look at [Smashing Magazine](https://www.smashingmagazine.com/2010/01/color-theory-for-designers-part-1-the-meaning-of-color/) for a introduction to color theory
* Grid system - This [article](https://webdesign.tutsplus.com/articles/a-comprehensive-introduction-to-grids-in-web-design--cms-26521) provides a comprehensive overview on the grid system
* Typography - Here is an [article](https://www.springboard.com/blog/best-resources-typography-design-online/) that has a list of resources to get you started on typography
* Iconography - Take a look at [Metro Studio](https://www.syncfusion.com/downloads/metrostudio) for free icons.
* Visual Hierarchy - This [article](https://www.awwwards.com/understanding-web-ui-visual-hierarchy.html) covers the different techniques used to create visual hierachies
* Flat vs Skeuomorphism - This [article](https://www.webinsation.com/flat-design-some-good-some-bad/) provides the good and bad of flat design

## UX Design

At it's core, UX Design is about understanding both user and business goals, and tailoring a product that strikes a right balance between both goals within given constraints such as budget or time.

Just like Software Development, UX Design is an iterative process. It involves many stages like researching, planning, testing, and at each stage are many different techniques that you could use. A good place to find these techniques would be the [UX Project Checklist](https://uxchecklist.github.io/) created by Andrea Soverini.

## Which is More Important?

The metaphor of building a house by Clayton Yan (UX Designer at UserTesting) illustrates this aptly. UX is the structure (i.e. the number of rooms, one-storey or two-storey and where the front door leads). Meanwhile, UI is about the visual (i.e. whether the same wallpaper or vases is used). A good UX with bad UI is like an ugly house with nice structure. A good UI with bad UX is like a beautiful house with the front door leading straight to the bathroom. To sum it up, UI and UX are **equally** important in creating an awesome experience for the users. 

## The Process of UX Design

As stated above, UX is a highly iterative process to achieve a fine balance between meeting the needs of the target audience and achieving business goals. Many times the functionality of a product is prioritised over the experience it gives. However this should not be the case when experience is the deciding factor in delighting or frustrating users.

Catering the right experiences for users does not have to be difficult. The stages of UX Design are logical and can be decomposed into four main stages as shown, with each section supplemented with what you would do during each stage, and the expected deliverables at the end of each stage.
1. Preliminary Research
1. Prototyping
1. User Testing
1. Maintenance

### Preliminary Research
The first stage is Preliminary Research. This is where you would:
  * Understand your target audience through [Contextual Inquiry](https://www.usabilitybok.org/contextual-inquiry), which involves interviewing users to find out about their wants, needs and pain-points, in the setting of them using the actual product and/or products from competitors
  * Gather the tools needed for the project such as wireframing or prototyping [tools](https://medium.com/@Mockplus/the-10-best-wireframing-and-prototyping-tools-for-designers-a808e81ecadf) for creating low to high fidelity products for testing
  * Depending on whether the project is self-initiated (taking a known product and re-designing it) or client-based (working for businesses or non-profits), discuss and create a project plan with stakeholders

#### Deliverables
  * [Personas](https://static1.squarespace.com/static/519deddfe4b07b846eef9842/55915575e4b0a919cf5a0da5/55a5b693e4b0576c4f936a78/1436923548676/11_all_with_eps_brands.jpg?format=750w) - User profiles that are representative of the target audiences
  * [User Stories](https://i.pinimg.com/originals/00/ac/37/00ac379ca5b9c28de3d56bdea4580e2e.jpg) - Representation of the most important user actions or motivations for each persona
  * [User Scenarios](http://www.davedoyle.com/prof/portfolio/images/large/petcare_scenario.gif) - Case studies of how users use the products, supplementing User Stories to allow you to better empathize with the users
  * [User Flows](https://cdn-images-1.medium.com/max/1600/0*aO_6rai_fTnSe5rh.) - Depiction of how users navigate through the product for each scenario
  
### Prototyping
After Preliminary Research, the next stage is Prototyping. This is where you would:
  * [Test](https://usabilitygeek.com/usability-testing-prototypes/) whether the product flow you came up with is smooth
  * Create [prototypes](https://www.mockplus.com/blog/post/wireframe-mockup-prototype-selection-of-prototyping-tools), or Minimum Viable Product (MVP), for further testing and refinement
  * Use studies gathered from the previous stage to get a better sense of how users would interact with the final product, which would be further refined in the User Testing stage
  
#### Deliverables
  * [Wireframes](https://wireframesketcher.com/samples/YouTube.png) - Depicts a skeleton version of an application, meant to develop a unified vision of content structure instead of focusing on asthetics; may or may not be clickable
  * [UI Elements](https://thumbs.dreamstime.com/z/flat-ui-design-elements-set-icons-buttons-progress-bars-vector-illustration-light-colors-33417705.jpg) - Basic visual components, which can start out basic and increase in quantity/quality with the fidelity of MVP
  * [Mockups](https://i0.wp.com/brandhorse.com/wp-content/uploads/2016/01/EZ-Frabic-Mobile-App-UX-UI-Design.jpg?resize=960%2C750) - Built upon an agreed version of wireframe with higher fidelity and asthetics
  
### User Testing
After Prototyping, the next stage is User Testing. This is where you would:
  * Have a clear sense of the user goals at this stage
  * Use a low or high fidelity prototype during [usability testing](https://www.uxbooth.com/articles/usability-testing-dont-guess-test/) to determine product effectiveness
  * Follow an [iterative design process](https://www.interaction-design.org/literature/article/design-iteration-brings-powerful-results-so-do-it-again-designer) to re-prototype after gathering user feedback
  
#### Deliverables (before actual testing with users)
  * [Usability Test Plan](https://www.smileycat.com/wp-content/uploads/2016/05/usability-test-plan-dashboard.png) - Summary of background, goals, and test methodology
  * [User Testing Script](http://www.ariadne.ac.uk/images/issue62-loureiroKoechlin/CeciliaLoureiroKoechlin-03-small.gif) - What to say as the facilitator, encouraging users to think out loud during the test
  
#### Deliverables (after actual testing with users)
  * [Usability Reports](https://image.slidesharecdn.com/january2012nhupa-120206210831-phpapp02/95/delivering-results-how-do-you-report-user-research-findings-18-728.jpg?cb=1374224669) - Thorough introduction to what aspects of the product was or was not effective and how to improve
  * [Journey Maps](https://cdn-images-1.medium.com/max/1600/1*jAfZXNWAx50fWEvoF-eWLw.png) - More detailed user flow based on how users are observed to use the product in actual testing conditions
  * [Refined Prototype](https://cdn-images-1.medium.com/max/1600/1*8qPOD1DG19Kj2sfJkTk3Eg.png) - A more refined prototype that is closer to meeting user expectations, each iteration should always be tested before moving to a higher fidelity
  
### Maintenance
After User Testing and the launch of the final product, the next stage is Maintenance. This is where you would:
  * Follow-up with the product after the final product launch
  * Recursively update, revise and maintain the content in the application
  
#### Deliverables
  * [Sitemaps](https://www.kristenjoybaker.com/uploads/1/3/7/6/13760055/3563790_orig.jpg) - Depiction of how all pages of the product interconnect
  * [Taxonomies](https://blog.fuzzymath.com/wp-content/uploads/2015/09/UCAN_Taxonomy-1024x775.png) - Organized depiction of how features or information in the product relate to one another
  * [Content Governance Plans](https://image.slidesharecdn.com/contentstrategyin2015-150706154016-lva1-app6891/95/slide-43-1024.jpg) - Complete strategy for updating, revising and maintaining all content within the product
  
### Ending Notes
The clearly-stated objectives and deliverables of each stage culminate in a well-defined target audience and a well-designed prototype needed to match the expectations of that audience. UX Design requires empathy in the users' shoes and effort to address their problems, however all that is worth it in the grand scheme of delighting users when they use your applications.
  

</div>
