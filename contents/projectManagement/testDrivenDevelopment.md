<frontmatter>
  title: Test-Driven Development
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

# Test-Driven Development

Author: [Yash Chowdhary](https://github.com/yash-chowdhary)

Reviewers: [Ang Ze Yu](https://github.com/ang-zeyu), [Neil Brian](https://github.com/nbriannl), [James Pang](https://github.com/jamessspanggg)

<box id="article-toc">

* [Test-Driven Development](#test-driven-development)
  * [What is Test-driven Development](#what-is-test-driven-development)
    * [Development Cycle](#development-cycle)
  * [Why adopt Test-Driven Development?](#why-adopt-test-driven-development)
    * [Benefit 1: Improves Code Quality](#benefit-1-improves-code-quality)
    * [Benefit 2: Ensures that Product Requirements are Understood](#benefit-2-ensures-that-product-requirements-are-understood)
    * [Benefit 3: Increases Productivity](#benefit-3-increases-productivity)
  * [Caveats and Limitations](#caveats-and-limitations)
  * [Advanced Test-Driven Development](#advanced-test-driven-development)
  * [Getting Started with TDD](#getting-started-with-tdd)
    * [Basic](#basic)  
    * [Intermediate](#intermediate)
    * [Advanced](#advanced)
  * [References](#references)

{.px-3}

</box>

## What is Test-driven Development? 

Typically, when you write unit tests, you test out the functionality of code you've already written. Test-driven development (a.k.a TDD) is a software development process that emphasizes the opposite - writing tests before production code. It is a design technique aimed at understanding requirements before writing functional code.

### Development Cycle

Robert Martin describes the "Three Laws of TDD" as - 
> "First Law: You may not write production code until you have written a failing unit test. 
>
> Second Law: You may not write more of a unit test than is sufficient to fail, and not compiling is failing. 
>
> Third Law: You may not write more production code than is sufficient to pass the currently failing test.”

Figure 1 below shows the general TDD cycle.

<center>
<pic src="images/tddSteps.jpg" alt="tddSteps" height="650">

_Figure 1. General TDD cycle._[^2]
</pic>
</center>

Kent Beck, creator of Xtreme Programming and advocate of TDD, states that the general development cycle of TDD is as follows[^1]:

1. **Add a test**

    In this phase of the cycle, all you need to focus on is writing tests. Understanding the product's/feature's requirements and specifications is crucial at this stage of the development cycle. 

1. **Run the test**

    In this phase, you check if the new test added fails. This is an important step as it shows that the new test does not pass without requiring new code, and also rules out false positives (i.e. the test passes when it shouldn't). Furthermore, the reason for its failure must be clear. 

1. **Write functional code**

    Here is when you write code to pass the test. The sole aim at this point is to pass the test and to write code whose functionality is restricted to the scope of the test. It is possible that the code may not be in its best form (style- and syntax-wise), but that's something you need not worry about at this stage.

1. **Re-run test(s)**

    Re-running the test suite and ensuring the test passes not only helps in keeping the cycle running, but also in increasing your confidence in the code that you have just written. This is because it shows that the new code meets the test requirements and does not break any existing features/requirements. 

1. **Refactor the code**

    As and when you write more production code and test to make sure it meets all your business, product, and/or feature requirements, it's important that the code itself meets a certain standard. Improving code style and readability, employing design patterns, and reducing duplication of code are some of the [many things](https://refactoring.com/catalog/) you can work on.


Steps 1-5 outline one **iteration** of the TDD cycle. In practice, several such iterations are required - sometimes even to get the same test or portion of the test to pass.

The essence of TDD can therefore be summarized into the mnemonic "Red, Green, Refactor", where "Red" signifies failure and "Green" signifies success (of the test). "Refactor" refers to the refactoring step in the TDD cycle.

<center>
<pic src="images/mantra.jpg" alt="mantra" height="240">

_Figure 2. Red, Green, Refactor._[^2]
</pic>
</center>

## Why adopt Test-Driven Development?

Some reasons why teams can choose to adopt the approach of test-driven development are: 

### Benefit 1: Improves Code Quality

TDD can lead to well-written code. This is a direct consequence of the cycle that was explained [above](#development-cycle). One of the best practices while following TDD is to focus on small units of code (methods, classes, modules, etc). This can lead to more modular, flexible code with looser coupling that can be scaled[^3]. This way the code unit targets a specific requirement or feature with just enough code to fail, and descriptive enough for a new comer to understand it.

Additionally, developers will work on units of code that can be written and tested independently and integrated later. This can be achieved through <trigger trigger="click" for="modal:index-mock">mock frameworks</trigger>, which help drive home the rationale of writing unit tests.

The tests also cover all possible branches and paths that the production code can take because no more code is written than is necessary[^3]. Detecting bugs at early stages is easier when you're working with code that has high coverage [^5]. 

<modal title="Mock Objects and Frameworks" id="modal:index-mock">
Mock objects are simulated objects whose sole purpose is to act as a black box that mimics the behaviour of real-world objects during testing. They are especially useful in writing unit tests, which are meant to test the functionality of a portion of code assuming that the rest of the code behaves as it should. 
</modal>

### Benefit 2: Ensures that Product Requirements are Understood

Test-driven development requires the developers to have a good understanding of the product or feature requirements before any sort of development can begin. As mentioned earlier, the first step in development is writing tests. Coming up with tests without a clear picture of the requirements or specifications  can be dangerous as: 

1. you could end up testing something that doesn't need to be tested, or 
1. you could end up overlooking some requirements that _must_ be tested, thus giving you a false sense of accomplishment when you write production code to pass the test

TDD thus prevents the team from rushing into development and pushes them to prioritize the requirements-gathering step. 



### Benefit 3: Increases Productivity

Hakan Erdogmus, Professor of Software Engineering at Carnegie Mellon University, found that programmers who wrote more tests tended to be more productive^[Erdogmus, 2005. ["On the Effectiveness of Test-first Approach to Programming"](https://web.archive.org/web/20141222180731/http://nparc.cisti-icist.nrc-cnrc.gc.ca/npsi/ctrl?action=shwart&index=an&req=5763742&lang=en)], and since employing TDD meant writing more tests, there is a correlation between productivity and TDD.

Employing TDD also lays out the design of a program. It forces programmers to think about coming up with failing tests and working their way up to writing code that passes the test.

Furthermore, since tests are written first, there is close to 100% coverage. This ensures that production code is covered by at least one test. This can boost individual and team morale, confidence in the code, and productivity as a consequence.


## Caveats and Limitations

Even with some of the attractive benefits of adopting TDD, there is no one-size-fits-all solution for project design management. Some of the limitations of TDD are: 

1. **Strict emphasis on communication of requirements and specifications**

    Product/Feature requirements are crucial to teams that employ a TDD-style approach to development. As mentioned earlier, the very first step of the development cycle is to write acceptance tests. If the requirements are not communicated properly, teams will end up constantly having to change their test suites. In extreme scenarios, a re-write may be necessary.

    As a result, a lot of man hours can go into making sure the requirements are in order -  consequently, this could result in a slow start to development.

2. **Tough to incorporate TDD into <trigger trigger="click" for="modal:index-legacy">legacy projects [^6]</trigger>**

    Legacy projects are hard to maintain even without having to incorporate TDD into them. As these projects have a history of design and implementation choices made based on the requirements and/or the resources at the time, investing man hours in revamping or "modernizing" them is not always deemed as top priority. 

    <modal title="Legacy Code" id="modal:index-legacy">
    Legacy code refers to an application system source code type that is no longer supported. Legacy code can also refer to unsupported operating systems, hardware and formats. In most cases, legacy code is converted to a modern software language and platform. However, to retain familiar user functionality, legacy code is sometimes carried into new environments. 
    </modal>

    When you bring TDD into the picture, things get more complex as failing tests are written at first with the assumption that production code will be written to ensure they pass. 
    
    Writing production code in legacy systems with the goal of passing a specific test is harder than it seems. For instance, there may be dependencies between components that need to be "broken" for them to be testable, which could result in  several workarounds. This could potentially add on to the complexity of the project, making it counter-productive as the entire point of the refactoring step is to make the code more readable[^4].

3. **High Maintenance**
    
    Maintaining test-suites becomes a part of the overall overhead of the project. Since TDD-style approaches rely heavily on tests, there is a need for the tests to be in proper shape in order to prevent a false sense of correctness. 
    
    TDD also has a steep learning curve. The entire process takes time to get accustomed to. Refactoring of code by using good design patterns comes with time, experience, and a lot of practice.

4. **Great Importance given to Unit Tests**    

    Unit tests are generally much more in number than other sort of tests like integration, system, or interface tests. Developers run the risk of giving less importance to these classes of tests if there are a high number of passing unit tests[^3].

## Advanced Test-Driven Development

Advanced practices of test-driven development can lead you to <tooltip content="Acceptance Test-Driven Development">ATDD</tooltip> and consequently <tooltip content="Developer Test-Driven Development">DTDD</tooltip>[^2]:

* ATDD : Acceptance TDD involves the use of <trigger trigger="click" for="modal:index-acceptance">acceptance tests</trigger> or behavioural specifications in its workflows. By following Acceptance TDD, a detailed specification of the requirements of your product can be obtained.

* Developer TDD: This is the form of development that actually involves writing tests followed by <tooltip content="No more; No less">minimal</tooltip> production code that passes the test. This helps in achieving the goal of a detailed specification of the executable design for your solution. DTDD is often simply called TDD.

<modal title="Acceptance Tests" id="modal:index-acceptance">
Acceptance tests, also called Customer Acceptance tests, are tests that describe the behavior of a software product. They are tests that have the power to encode the technical and non-functional requirements, business logic, and detailed usage requirements of the product. They are similar to unit tests in that they have binary results - pass or fail. 
</modal>

Shown below is the figure that depicts a combination of ATDD and DTDD.

Ideally, you start off by writing acceptance tests and ensuring that they pass. The DTDD cycle is used to write code for those tests that fail.

<center>
<pic src="images/effectiveSteps.jpg" alt="effectiveSteps" height="700">

_Figure 3. Combination of ATDD and DTDD._[^2]
</pic>
</center>


## Getting Started with TDD

As explained above, test-driven development is a software design paradigm. While it does have an established and unelaborate framework, there are elements of the development cycle that -
1. could be new to you (such as testing and mock frameworks), or 
1. you require practice in (such as refactoring code)

### Basic

If you're new to testing, or if you're not completely sold on why software testing is important, check out these 2 articles to get a deeper appreciation for the subject - ['Understanding Software Testing'](https://medium.com/@netxm/how-to-get-started-with-software-testing-9fa1ce4f2a64) and ['The Importance of Software Testing'](https://www.testdevlab.com/blog/2018/07/importance-of-software-testing/).

For those of you who'd like to get your hands dirty, here are a couple of tutorials on getting started with unit testing with Java - one using the [TestNG framework](https://dev.to/chrisvasqm/introduction-to-unit-testing-with-java-2544), and another using the [JUnit framework](https://www.guru99.com/junit-tutorial.html). They are both comprehensive tutorials - covering topics from setting up your environment for testing and writing basic tests to annotations. 

### Intermediate

Now that you're familiar with the idea of testing and the basics of writing unit tests, it is time to shift your focus to using fakes and mocks. As mentioned before, mock objects are simulated objects whose sole purpose is to act as a black-box that mimics the behaviour of real-world objects. The main use case of these is in unit tests, where the focus is on testing a *unit* of code. In order to do this, it's best to avoid introducing dependencies on external components. Isolation is key. 

To read more about fakes, mocks and stubs, refer to [this article](https://blog.pragmatists.com/test-doubles-fakes-mocks-and-stubs-1a7491dfa3da).

TutorialsPoint's [<tooltip content="Mockito is a Java-based mocking framework.">Mockito</tooltip> Tutorial](https://www.tutorialspoint.com/mockito/index.htm) is a great place to get started with mocking. 

Here are some resources for other programming languages that may be useful -  
1. Test framework tutorials:
    * [pytest (Python)](https://docs.pytest.org/en/latest/)
    * [Jest (NodeJS, React, Angular, Vue)](https://jestjs.io/docs/en/getting-started)
1. Mock Framework tutorials:
    * [mock (Python)](https://docs.python.org/3/library/unittest.mock.html)
    * [SinonJs (Javascript)](https://sinonjs.org/)

### Advanced

At this juncture, you know how to write tests and how to use test doubles. What's left is to really apply this knowledge to [writing testable code]({{baseUrl}}/contents/testing/writing-testable-code.html). The article ['How to Write Testable Code and Why It Matters'](https://www.toptal.com/qa/how-to-write-testable-code-and-why-it-matters) is another good read. These articles discuss design principles and general rules of thumb to follow while writing write functional code. These principles are language-agnostic. 

The last step of the TDD cycle is refactoring your code. There is more to refactoring than just renaming variables and extracting methods. Martin Fowler's [refactoring.com](https://refactoring.com/catalog/) catalogs a whole array of refactoring principles and tips that can be very handy - even to the most experienced of developers.

['Test-Driven Development: By Example'](https://www.amazon.sg/Test-Driven-Development-Kent-Beck/dp/0321146530/ref=sr_1_1?tag=pbourgau-20&s=books&ie=UTF8&qid=1495080564&sr=1-1&keywords=tdd+by+example) by Kent Beck is a worthwhile read. Meant to inspire developers to embrace TDD, this book discusses the crux of the approach along with best practices, techniques and sample projects.

## References

[^1]: Beck, K. 2000. ["Extreme Programming Explained"](https://dl.acm.org/doi/book/10.5555/318762)
[^2]: [Agile Data, Scott W. Ambler](http://agiledata.org/essays/tdd.html)
[^3]: [Test-Driven Development - Wikipedia](https://en.wikipedia.org/wiki/Test-driven_development)
[^4]: Smart, J.F. 2009. ["An Introduction to Test-Driven Development with Legacy code"](https://dzone.com/articles/introduction-test-driven)
[^5]: P. S. Kochhar, F. Thung and D. Lo, 2015. ["Code coverage and test suite effectiveness: Empirical study with real bugs in large systems"]((https://doi.org/10.1109/SANER.2015.7081877))
[^6]: [Legacy Code](https://www.techopedia.com/definition/25326/legacy-code)

</div>