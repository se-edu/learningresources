<frontmatter>
  title: Accessibility
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Design Systems

**Author(s): [Tan Heng Yeow](https://github.com/tanhengyeow)**

Reviewers: [Ronak Lakhotia](https://github.com/RonakLakhotia), [Metta Ong](https://github.com/ongspxm)

## What is a Design System

The following definition best encapsulates the essence of a Design System.
> A Design System is the Single Source of Truth which groups all the elements that will allow the teams to design, realize and develop a product. <sub>--[Blog post from UX Collective](https://uxdesign.cc/everything-you-need-to-know-about-design-systems-54b109851969)</sub>

The typical structure of a Design System looks like this:
<center>
<img src="https://cdn-images-1.medium.com/max/2600/1*wSHUJh069L618oXqVZYDtA.png" alt="Typical structure of a Design System" width="95%">

_Figure 1. Typical structure of a Design System_ ([source](https://uxdesign.cc/can-design-systems-fix-the-relationship-between-designers-developers-eb12fc9329ab))
</center>

Below is a brief explanation of the parts that make up a Design System. The explanation includes reference to a living example, [Polaris](https://polaris.shopify.com/), Shopify's Design System.
- **Rules**: Rules include abstract elements such as brand values, mindset, and shared beliefs. For example, one of Polaris' [product experience principles](https://polaris.shopify.com/patterns-and-guides/product-experience-principles#navigation) is to *Put merchants first*. The principle promotes thinking about the needs of different types of merchants and be intentional about how to respond to them in the product or feature.
- **Pattern Library**: A Pattern Library integrates functional components and their usage. For example, Shopify's [product component library](https://github.com/Shopify/polaris-react) contains a custom [Datepicker](https://github.com/Shopify/polaris-react/tree/master/src/components/DatePicker) component for merchants to select a date range.
- **Building Blocks**: Building Blocks refer to documentation consisting of the collection of interface elements. Polaris has a section which describes their [components](https://polaris.shopify.com/components/get-started) in detail. For example, the custom [Datepicker](https://polaris.shopify.com/components/forms/date-picker#navigation) component is clearly documented with different examples.
- **Style Guide**: A Style Guide focuses on graphic styles such as colors, fonts, illustrations, etc and their usage. For example, Polaris' [style guide](https://polaris.shopify.com/design/colors#section-color-usage) recommends to use Indigo for buttons and avoid using Indigo for text links.

## Why should we use a Design System?

<center>
<img src="https://cdn-images-1.medium.com/max/2600/1*wFOwLNi-xGBfyGnssynQaQ.png" alt="Typical Design and Development process" width="95%">

_Figure 2. Typical Design and Development process_ ([source](https://uxdesign.cc/can-design-systems-fix-the-relationship-between-designers-developers-eb12fc9329ab))
</center>

Consider the typical design and development process. Designers come up with sketches, wireframes and hand them over to the developers to implement them through code. Here's the problem:

Designers are looking at multiple directions and solutions, constantly comparing and tweaking their design. On the other hand, developers are ultimately accountable for shipping code. They focus on making sure that the correct architecture is in place, thinking about security and the performance of the product, hoping that the designers already have all the design details thought out.

Over time, it is likely that many teams working on different parts of the product will create UI/UX design inconsistencies in the product. For example, HubSpot [explained how they found UI/UX design inconsistencies](https://product.hubspot.com/blog/how-building-a-design-system-empowers-your-team-to-focus-on-people-not-pixels) in their interface after auditing their User Interface (UI) components.

Here is another example of a conversation held at [Modus Create](https://moduscreate.com/), a digital product agency, highlighting a similar problem.

<center>
<img src="https://3lhowb48prep40031529g5yj-wpengine.netdna-ssl.com/wp-content/uploads/2018/07/chat.jpg" alt="Conversation at Modus Create" width="75%">

_Figure 3. Conversation at Modus Create_ ([source](https://moduscreate.com/blog/design-systems-and-how-your-company-benefits-from-them/))
</center>

## Why not front-end frameworks or style guides?
Front-end frameworks such as [Bootstrap](https://getbootstrap.com/) have reusable components that save a lot of time and effort. However, there are a few disadvantages:
1. **Not suitable for designers**: [Sketch](https://www.sketch.com/) is an industry standard tool used by designers for UI design. Most front-end frameworks do not have relevant *.sketch* or source files that allow them to change the design of components in the framework.
2. **Not suitable for extensive customization**: Products with a distinctive level of identity require additional development effort, which defeats the point of using a framework.
3. **Not suitable for apps that emphasize on performance**: Front-end frameworks come with elements that the team may not need. The unused code could reduce overall performance.

<box type="info">
Note that the style guide mentioned here is a term used in the past when Design Systems had not been explored yet. The Style Guide of a Design System is more focused and emphasizes on graphic styles only.
</box>

Style guides present components that can be used quickly in mockups. This is an example of Tor's [style guide](https://styleguide.torproject.org/components/).  However, there are a few disadvantages:
1. **Not easily traceable**: It is hard to trace where the components came from as different types of components may be built over time by different teams.
2. **Not easily documented**: Most of the time, style guides lack documentation on what each component does. Also, too much documentation may decrease reusability of components because it is infeasible for designers/developers to go through a lot of pages to find a component they wish to use.

## Benefits of a Design System
Design Systems combine the good parts of front-end frameworks and style guides.

**Benefit 1: Shared Common language**

Design Systems act as a [Single Source of Truth](https://en.wikipedia.org/wiki/Single_source_of_truth). This allows the whole organization to access changes and updates. Any member of a team would have complete documentation of design mock-ups at their fingertips. This results in fewer misunderstandings and much better implementation of designs.

Design Systems also [elimate knowledge silos](https://moduscreate.com/blog/design-systems-and-how-your-company-benefits-from-them/). There is a lesser risk of context gaps being formed because information is shared across teams.

**Benefit 2: Reusability**

Design Systems provides a shared library of reusable components. When every component in a design system is reusable, teams can use these reusable components to quickly assemble and implement new pages.

As a result, this process substantially decreases the possibility of teams having to duplicate components, which means extra resources for focusing on more valuable things such as customer experience.

**Benefit 3: Faster Development**

When products are integrated with a Design System, updates are only required to be carried out in one place. This [blog post](https://moduscreate.com/blog/design-systems-and-how-your-company-benefits-from-them/) from Modus Create explains how Design Systems helped them to achieve faster development speed.

In a [blog post](https://airbnb.design/building-a-visual-language/) from Airbnb, the team explained how they were able to create nearly 50 screens within just a few hours by using their Design System. It was also mentioned by the team that they build and release features on all native platforms at roughly the same time now. Development is faster for them now since product engineers can focus more on writing the feature logic rather than code that is responsible for the presentation layer of the application.

## How to build a Design System

There are paid solutions out there to help organizations build Design Systems. However, there are open source tools to assist you in building your own Design System.

They are [Storybook](https://github.com/storybooks/storybook) and [React Styleguidist](https://github.com/styleguidist/react-styleguidist). Here is a good [blog post](https://blog.hichroma.com/storybook-vs-styleguidist-2bd93d6dcc06) describing the difference between these tools and how they complement each other in setting up a Design System.

**Storybook**

<center>
<img src="https://raw.githubusercontent.com/storybooks/storybook/next/media/storybook-intro.gif" alt="Storybook" width="95%">

_Figure 4. Storybook_ ([source](https://github.com/storybooks/storybook))
</center>

[Storybook](https://github.com/storybooks/storybook) is a development environment for UI components. It helps you build UI components in isolation and offers handy features to visualize any mock data you supply. It helps in setting up the pattern library of a Design System.

**React Styleguidist**

<center>
<img src="https://camo.githubusercontent.com/0ded3b0835a4ace4664a6833985affbde783ed47/68747470733a2f2f64337676366c703535716a6171632e636c6f756466726f6e742e6e65742f6974656d732f303631663041326e3142304833703054317031662f72656163742d7374796c65677569646973742d6c6f676f2e706e67" alt="React Styleguidist" width="60%">

_Figure 5. React Styleguidist_ ([source](https://github.com/styleguidist/react-styleguidist))
</center>

[React Styleguidist](https://github.com/styleguidist/react-styleguidist) simplifies creating and maintaining a UI documentation site. It allows you to create pages in Markdown and import UI components. It helps in setting up the style guide of a Design System.

## Examples of existing Design Systems

This [website](https://designsystemsrepo.com/design-systems/) presents a comprehensive and curated list of design systems.

Out of the list, there are some noteworthy open-source Design Systems that you can use, contribute or take reference from:
1. [Material Design](https://material.io/design/): Created by Google. Components implemented from Material Design are found [here](https://github.com/material-components).
2. [Photon Design System](https://design.firefox.com/photon/): Used to build Firefox products. Components found [here](https://github.com/firefoxux).
3. [Lightning Design System](https://www.lightningdesignsystem.com/): Used to build products/apps related to Salesforce. The component library can be found [here](https://github.com/salesforce/design-system-react). Note that they use Storybook in setting up the pattern library.
4. [Shopify Polaris](https://polaris.shopify.com/): Used to build products/apps related to Shopify. The component library can be found [here](https://github.com/Shopify/polaris-react). Note that they use Storybook in setting up the pattern library.

## Summary

Design Systems keep things in order by acting as a Single Source of Truth, providing a common shared language across the organization. It encourages reusability of components, resulting in faster development time.
