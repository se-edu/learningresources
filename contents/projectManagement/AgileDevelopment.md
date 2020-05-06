<frontmatter>
  title: Agile Development
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Agile Development

Author: [Joanne Ong](https://github.com/joanneong)

<box id="article-toc">

* [Overview‎](#overview)
* [What is Agile Development?‎](#what-is-agile-development)
* [Why Adopt Agile Development?‎](#why-adopt-agile-development)
* [How to Adopt Agile Development?‎](#how-to-adopt-agile-development)
* [Caveats: What can Possibly go Wrong?‎](#caveats-what-can-possibly-go-wrong)
* [Putting it all Together: The Spotify Success Story‎](#putting-it-all-together-the-spotify-success-story)
* [Other Related Resources‎](#other-related-resources)
</box>

## Overview

In 2015, Forbes published an article boldly titled [Agile: The World's Most Popular Innovation Engine](https://www.forbes.com/sites/stevedenning/2015/07/23/the-worlds-most-popular-innovation-engine/#22b176267c76). Indeed, as a software engineering student, you may have heard of the term "agile development", "agile model" or even simply "agile" before. In this chapter, we will explore agile development in greater depth, and learn why agile development is still thriving today.

## What is Agile Development?

Popularised in 2001 by the [*Manifesto for Agile Software Development*](https://agilemanifesto.org/), agile development is essentially a software development approach that can be adopted to manage a software project.

Yet, you should not mistake agile development for any single method or framework (e.g. [Scrum](https://www.scrum.org/resources/what-is-scrum?gclid=EAIaIQobChMI2Y-34IDX2QIVwZWPCh3aUg7tEAAYASAAEgLPKvD_BwE)). Rather, you should see agile development as an overarching approach possessing several distinguishing characteristics as consolidated in the *Manifesto*. To further simplify these characteristics, agile development can essentially be described as an approach that is flexible, iterative and people-centric.

But what does "flexible, iterative and people-centric" really mean? To comprehend this, let us examine each characteristic individually.

#### 1. Agile Development is "Flexible"

For a start, agile development is "flexible" because it gives a project more room to evolve over time. This is possible as agile development utilises a form of planning known as [adaptive planning](https://www.projectmanager.com/blog/how-to-plan-in-an-agile-environment), which means that planning is done continuously throughout a project rather than sticking rigidly to a fixed plan established at the start. As a result, planning can take into consideration the project's current progress, and change accordingly to allow the project's priorities and scope to evolve.

To visualise this, let us consider adaptive planning in a game below:

>Imagine playing the old Battleships Game. You are using a traditional business strategy, which means that you have to select all your moves up front and then sit back and wait to see if you have won. I am using an agile business strategy, which means I can make a move, see what happens, learn from this, and adapt. Guess who is more likely to win?
>
>-- from [An agile, adaptive business strategy](https://agileforeveryone.com/2015/04/24/agile-adaptive-business-strategy-with-scrum/) by Edwin Dando

#### 2. Agile Development is "Iterative"

Secondly, agile development is "iterative" because it breaks product development work down into short cycles known as [iterations](https://www.agilealliance.org/glossary/iteration/). Each iteration involves going through planning, designing, coding and testing to result in a working product at the end of an iteration. However, a single iteration may not have added enough functionality to warrant a new release, so in reality multiple iterations are often required to release a new product or feature. Here is a diagram to illustrate this concept:

![Agile Development - Iterations](images/projectManagement-agileDev-iterations.jpg)

#### 3. Agile Development is "People-Centric"

Thirdly, agile development is "people-centric" because it focuses heavily on collaboration and effective communication. In fact, there are many recommendations on how to form a good agile team. While the specifics vary in accordance with the framework that you choose, the general consensus remains that agile development involves several crucial team roles that work in close relation with each other.

In particular, agile development also zooms in on direct and simple conversations as the main mode of communication. For example, popular agile frameworks like [Scrum](https://www.scrum.org/resources/what-is-scrum?gclid=EAIaIQobChMIrJLEjuPV2QIV1BuPCh17KwH_EAAYASAAEgKXPfD_BwE) and [Kanban](https://www.atlassian.com/agile/kanban) typically have daily meetings called stand-ups where participants meet for no more than 15 minutes while standing up. Each team member simply answers three questions: "what did I complete yesterday", "what will I work on today", and "am I blocked by anything?", quickly informing everyone of what is going on across the team.

The key characteristics then enable our software projects to gain numerous benefits when agile development is adopted to manage the project.

## Why Adopt Agile Development?

There are quite a number of reasons why you might want to adopt development, but here are three of the most pertinent reasons why:

1. **Agile development can increase user satisfaction with the product.**

    As a project and system unfolds, stakeholders often discover that what they originally requested for at the start of the project may not be what they really want. Luckily, since agile development is "flexible", the project’s priorities and scope can be readjusted to accommodate how the project is evolving. By the end of the project, the product will tend to stick closer to what stakeholders really want, increasing their satisfaction with the product.

2. **Agile development can increase product quality.**

    Under agile development, [testing](https://smartbear.com/learn/automated-testing/testing-in-agile-environments/) is part of every single iteration, so any bugs in the product can be found and fixed earlier and more easily. This can then result in increased product quality as bugs are kept to a minimum to ensure that working software is delivered at the end of each iteration.

3. **Agile development can increase project control.**

    Project control can be succinctly defined as the management of a project’s costs and schedule. With agile development’s iterative and people-centric nature, project managers can gain more opportunities to monitor what is going on in the project and assess the project’s progress more accurately. If need be, the project manager can also communicate with the client to decide whether to change the scope of the project. Even in the worst case scenario, agile development will still guarantee a working product delivered by the deadline since changes are made incrementally, eliminating the risk of total project failure.

## How to Adopt Agile Development?

Now that you have a glimpse of what agile development can offer you, you may be wondering, how can you adopt agile development in your software projects?

As introduced previously, agile development can actually be seen as a high-level concept (or abstraction) that describes certain principles to follow in a software project. As a result, different software projects can choose to implement agile development differently to fit their own circumstances.

That said, there are many agile development frameworks that can help to guide you along the way. Examples of popular agile frameworks include (but are not limited to):

- [Scrum](https://www.scrum.org/resources/what-is-scrum?gclid=EAIaIQobChMI2Y-34IDX2QIVwZWPCh3aUg7tEAAYASAAEgLPKvD_BwE)
- [Kanban](https://www.atlassian.com/agile/kanban)
- [Scrumban](https://www.agilealliance.org/what-is-scrumban/)
- [Lean Software Development](https://leankit.com/learn/lean/principles-of-lean-development/)
- [Extreme Programming (XP)](http://www.agilenutshell.com/xp)
- [Feature-Driven Development (FDD)](https://agilemodeling.com/essays/fdd.htm)
- [Dynamic Systems Development Method (DSDM)](https://dsdmofagilemethodology.wikidot.com/)

In addition to such agile frameworks, there are also many concrete practices that support agile development. These practices span over aspects like requirements, design, coding, testing and risk management, potentially easing your transition to agile development:

- [User Stories](https://www.agilemodeling.com/artifacts/userStory.htm)
- [User Story Mapping](https://www.thoughtworks.com/insights/blog/story-mapping-visual-way-building-product-backlog)
- [Backlogs](https://www.agilealliance.org/glossary/backlog/)
- [Velocity Tracking](http://www.softwaretestingstudio.com/agile-velocity-sprint-metrics/)
- [Timeboxing](https://www.telerik.com/blogs/the-importance-of-timeboxing-and-iterations-for-agile-planning)
- [Pair Programming](https://www.codementor.io/pair-programming)
- [Test Driven Development (TDD)](testDrivenDevelopment.html)
- [Continuous Integration](http://istqbexamcertification.com/what-is-continuous-integration-in-agile-methodology/)
- [Reflection loops](https://dzone.com/articles/reflection-loops-agile)

Of course, your project does not have to be utilising **all** the frameworks/concrete practices in order to be agile in its development. Instead, great care should be taken to analyse and select the appropriate framework/concrete practices that will work best for the project.

## Caveats: What can Possibly go Wrong?

Given that agile development can bring about so many benefits to our software projects, we must not forget that things can still go very wrong when using agile development. Such unfortunate situations are known as [agile anti-patterns](https://age-of-product.com/agile-management-anti-patterns/), or [agile smells](https://medium.com/agile-government-leadership/when-good-scrum-goes-bad-identifying-bad-agile-smells-690e7a16b501), and some common problems include:

- **Unfamiliarity with agile development**

    If the team is unfamiliar with agile techniques, you will not be able to reap the full benefits of agile development since agile development depends on tight collaboration between team members. Therefore, it is crucial to ensure that the entire team has good foundations in agile techniques before the transition to agile development. This can be achieved by investing early in training sessions for the team, and bring everyone in the team up-to-date with what agile development entails for the project.

- **Over-reliance on manual testing**

    If the team relies too much on manual testing, agile development will likely be a nightmare for the team. Recall that agile development is iterative, and that testing a part of every single iteration, so with merely manual testing, the team will be forced to spend an unreasonable amount of time and effort on testing alone. Consequently, it is standard practice to employ [automated testing](https://reqtest.com/agile-blog/you-cant-work-agile-without-automated-testing/) in agile development to allow developers and testers to focus on work that can be of higher value to the project.

- **Structural rigidity in larger organisations**

    If the team is operating in the context of a large organisation, agile development may not work out as well as it promises to. Unsurprisingly, transiting to agile development in large organisations poses great challenges because large organisations tend to have many established protocols and workflows in place. These protocols may then be familiar to the organisations but are not necessarily in line with agile principles and ideals, but discarding them entirely would cause huge disruptions to the organisations. To combat this, larger organisations can think about [hybrid models](https://www.agilealliance.org/what-is-hybrid-agile-anyway/) - combining Agile methods with other non-Agile techniques that the organisation is currently accustomed to.

## Putting it all Together: The Spotify Success Story

By now, you should have gained a healthy understanding of agile development: **what** it is, **why** we should adopt it, and **how** we can adopt it. As a conclusion, let us look at a small example of how [Spotify](https://www.spotify.com/sg-en/) - a Swedish music, podcast and video streaming service with over 70 million paying subscribers in 2018 - managed to apply agile development successfully to reap the benefits associated with agile development.

>In 2015, a small team in Spotify had an idea to solve a long-standing problem: how could users find the music they would really love in a library of millions of songs?...The team didn’t need a whole lot of ROI analyses or go up a steep hierarchical chain to get management approval to change the firm’s strategic plan. In an Agile setting, it was quick and easy for the team to carry out a series of tests. When the innovation, now known as Discover Weekly, was deployed just a few months later, it was a wild success—becoming not just a new feature but a global brand, resulting in an influx of millions of new users. The Discover Weekly team is just one of more than 100 small teams at Spotify, which has deployed Agile approaches to all work since its inception in 2008.
>
> -- from [Can big organizations be agile?](https://www.forbes.com/sites/stevedenning/2016/11/26/can-big-organizations-be-agile/#316f8d6238e7) by Steve Denning

[Having been agile since its inception in 2008](https://medium.com/project-management-learnings/spotify-squad-framework-part-i-8f74bcfcd761), Spotify has made full use of the fact that agile development is flexible, iterative and people-oriented to push out their new ideas efficiently. In this case, Spotify could steer the firm's strategy in a novel direction after sudden inspiration ("flexible"), and implemented the new feature in steps by "carry[ing] out a series of tests" ("iterative") while working together in a small group known as the *Discover Weekly* team ("people-oriented").

[And Spotify is not the only company to have benefitted from agile development](https://www.forbes.com/sites/stevedenning/2016/11/26/can-big-organizations-be-agile/#316f8d6238e7). Evidently, agile development has the potential to do much for your project and organization - but only after you learn how to unleash this potential proper.

## Other Related Resources

Here is a compilation of other resources on agile development for your further exploration:

#### Getting Started With Agile Development

- [How to start with agile development](https://saucelabs.com/blog/how-to-start-with-agile-development)
- [The Agile Coach](https://www.atlassian.com/agile)
- [The Beginner's Guide to Scrum and Agile Project Management](https://blog.trello.com/beginners-guide-scrum-and-agile-project-management)
- [Roles on Agile Teams: From Small to Large Teams](https://www.ambysoft.com/essays/agileRoles.html)
- [Scaled Agile Framework (SAFe)](https://www.scaledagileframework.com/agile-teams/)

#### Comparing Different Agile Frameworks

- [How to Pick the Right Agile Tool](https://www.smartsheet.com/how-pick-right-agile-tool)
- [Kanban vs Scrum](https://www.cprime.com/2015/02/3-differences-between-scrum-and-kanban-you-need-to-know/)
- [Scrum vs Kanban vs Scrumban](https://www.eylean.com/blog/2013/05/scrum-vs-kanban-vs-scrumban-planning-estimation-and-performance-metrics/)
- [Scrum vs Kanban vs Lean vs XP](https://dzone.com/articles/agile-framework-comparison-scrum-vs-kanban-vs-lean)
- [XP vs FDD vs FDSM](https://project-management.com/xp-fdd-dsdm-and-crystal-methods-of-agile-development/)

</div>
