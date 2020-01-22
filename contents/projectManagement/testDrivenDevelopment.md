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

## What is Test-Driven Development?

Test-driven development (a.k.a TDD) is a software development process that emphasizes writing tests before production code. To be more precise, it is a design technique.

Before we go right into the meat of the matter, it is important to understand what <tooltip content="Acceptance Test-Driven Development">ATDD</tooltip> and <tooltip content="Developer Test-Driven Development">DTDD</tooltip> are:

* ATDD : Acceptance TDD involves the use of <trigger trigger="click" for="modal:index-acceptance">acceptance tests</trigger> or behavioural specifications in its workflows. By following Acceptance TDD, a detailed specification of the requirements of your product can be obtained.

* Developer TDD: This is the form of development that actually involves writing tests followed by <tooltip content="No more; No less">minimal</tooltip> production code that passes the test. This helps in achieveing the goal of a detailed specification of the executable design for your solution. DTDD is often simply called TDD.

<modal title="Acceptance Tests" id="modal:index-acceptance">
Acceptance tests, also called Customer Acceptance tests, are tests that describe the behavior of a software product. They are tests that have the power to encode the technical and non-functional requirements, business logic, and detailed usage requirements of the product. They are similar to unit tests in that they have binary results - pass or fail. 
</modal>

### Development Cycle

Robert Martin describes the "Three Laws of TDD" as - 
> "First Law: You may not write production code until you have written a failing unit test. 
>
> Second Law: You may not write more of a unit test than is sufficient to fail, and not compiling is failing. 
>
> Third Law: You may not write more production code than is sufficient to pass the currently failing test.”

Kent Beck, creator of Xtreme Programming and advocate of TDD, states that the general development cycle is as follows:

1. **Add a test**

    In this phase of the cycle, all you need to focus on is writing tests. Understanding the product's/feature's requirements and specifications is crucial at this stage of the development cycle. 

1. **Run the test**

    In this phase, you check if the new test added fails. This is an important step, as it shows that the new test does not pass without requiring new code because the required behavior already exists, and also rules out false positives (i.e. the test passes when it shouldn't). Furthermore, the reason for its failure must be clear. 

1. **Write functional code**

    Here is when you write code to pass the test. The sole aim at this point is to pass the test and to write code whose functionality is restricted to the scope of the test. It is possible that the code may not be in its best form (style- and syntax-wise), but that's something you need not worry about at this stage.

1. **Re-run test(s)**

    Re-running the test suite and ensuring the test passes not only helps in keeping the cycle running, but also in increasing your confidence in the code that you have just written. This is because it shows that the new code meets the test requirements, and does not break any existing features/requirements. 

1. **Refactor the code**

    As and when you write more production code and test to make sure it meets all your business, product, and/or feature requirements, it's important that the code itself meets a certain standard. Improving code style and readability, employing design patterns, and reducing duplication of code are some of the [many things](https://refactoring.com/catalog/) you can work on.

1. **Repeat** 

    Steps 1-5 outline one **iteration** of the TDD cycle. In reality, several such iterations are required - sometimes even to get the same test or portion of the test to pass.

Figure 1 below shows the general TDD (or DTDD, whichever you prefer) cycle as a UML activity diagram. 

<center>
<pic src="images/tddSteps.jpg" alt="tddSteps" height="420">

_Figure 1. General TDD cycle._
</pic>
</center>

Shown below is the UML activity diagram that depicts a combination of ATDD and DTDD. 

Ideally, you start off by writing acceptance tests and ensuring that they pass. The DTDD cycle is used to write code for those tests that fail.

<center>
<pic src="images/effectiveSteps.jpg" alt="effectiveSteps" height="500">

_Figure 2. Combination of ATDD and DTDD._
</pic>
</center>

The essence of TDD can therefore be summarized into the mnemonic "Red, Green, Refactor", where "Red" signifies failure and "Green" signifies success (of the test). "Refactor" refers to the refactoring step in the TDD cycle.

<center>
<pic src="images/mantra.jpg" alt="mantra" height="170">

_Figure 3. Red, Green, Refactor._
</pic>
</center>

## Why adopt Test-Driven Development?

Some reasons why teams can choose to adopt the approach of test-driven development are: 

### Benefit 1: Test-driven Development Improves Code Quality

TDD can lead to well-written code. This is a direct consequence of the cycle that was explained [above](#development-cycle). One of the best practices while following TDD is to focus on small units of code. This can lead to more modular, flexible code with looser coupling that can be scaled. Additionally, developers will work on units that can be written and tested independently and integrated later. This can be achieved through <trigger trigger="click" for="modal:index-mock">mock frameworks</trigger>, which help drive home the rationale of writing unit tests.

The tests also cover all possible branches and paths that the production code can take because no more code is written than is necessary. Detecting bugs at early stages is possible when you're dealing with code that has high coverage.

<modal title="Mock Objects and Frameworks" id="modal:index-mock">
Mock objects are simulated objects whose sole purpose is to act as a black-box that mimics the behaviour of real-world objects during testing. They are especially useful in writing unit tests, which are meant to test the functionality of a portion of code assuming that the rest of the code behaves as it should. 
</modal>

### Benefit 2: Test-driven Development Ensures that Product Requirements are Understood

Test-driven development requires the developers to have a good understanding of the product or feature requirements before any sort of development can begin. As mentioned earlier, the first step in development is (ideally) writing acceptance tests. Coming up with acceptance tests without a clear picture of the requirements or specifications  is meaningless as - 

_a)_ you could end up testing something that doesn't need to be tested, thus giving you a false sense of accomplishment when you write production code to pass the test, or 

_b)_ you could end up overlooking some requirements that _must_ be tested, which is far worse.

TDD thus prevents the team from rushing into development, and pushes them to prioritize the requirements-gathering step. 



### Benefit 3: Test-Driven Development can Increase Productivity

[Erdogmus, 2005](https://web.archive.org/web/20141222180731/http://nparc.cisti-icist.nrc-cnrc.gc.ca/npsi/ctrl?action=shwart&index=an&req=5763742&lang=en) found that programmers who wrote more tests tended to be more productive, and since employing TDD meant writing more tests, there is an indirect correlation between productivity and TDD.

Employing TDD also lays out a design of a program. It forces programmers to think about coming up with failing tests and working their way up to writing code that passes the test. Furthermore, since tests are written first, there is close to 100% coverage and ensures that production code is covered by atleast one test. This boosts individial and team morale, confidence in the code, and productivity as a consequence.


## Caveats and Limitations

Even with some of the attractive benefits of adopting TDD, there is no one-size-fits-all solution for project design management. Some of the limitations of TDD are: 

1. **Strict emphasis on communication of requirements and specifications**

    Product/Feature requirements are crucial to teams that employ a TDD-style approach to development. As mentioned earlier, the very first step of the development cycle is to write acceptance tests. If the requirements are not communicated properly, teams will end up constantly having to change their test suites. In extreme scenarios, a re-write may be necessary.

    As a result, a lot of man hours can go into making sure the requirements are in order -  consequently, this could result in a slow start to development.

2. **It is tough to incorporate TDD into legacy projects**

    Legacy projects are hard to maintain even without having to incorporate TDD into them. As these projects have a history of design and implementation choices made based on the requirements and/or the resources at the time, investing man hours in revamping or "modernizing" them is not always deemed as top priority. 

    When you bring TDD into the picture, things get more complex as failing tests are written at first with the assumption that production code will be written to ensure they pass. Writing production code in legacy systems with the goal of passing a specific test is harder than it seems, and could involve several workarounds because of design and implementation restrictions. This could potentially add on to the complexity of the project.

    TDD could be a good option for greenfield projects, but probably not for legacy projects.

3. **High Maintenance**
    
    Maintaining test-suites becomes a part of the overall overhead of the project. Since TDD-style approaches rely heavily on tests, there is a need for the tests to be in proper shape in order to prevent a false sense of correctness. TDD also has a steep learning curve. The entire process takes time to get accustomed to. Refactoring of code by using good design patterns comes with time, experience, and a lot of practice.

    Furthermore, there is an extensive use of unit tests using mock frameworks, which could lead to less importance given to integration, system, or interface tests. 


## Getting Started with TDD

Since TDD is an approach which has a well-established framework in place, you can get started with it by just following the steps that were discussed [above](#development-cycle). 

Here are some resources that may be useful -  

1. [Writing Tests](https://www.toptal.com/qa/how-to-write-testable-code-and-why-it-matters) - discusses the what, why and how of unit-testing with easy-to-follow tutorials (in C#).
1. Test framework tutorials:
    * [JUnit (Java)](https://junit.org/junit5/docs/current/user-guide/)
    * [pytest (Python)](https://docs.pytest.org/en/latest/)
    * [Jest (NodeJS, React, Angular, VueJS)](https://jestjs.io/docs/en/getting-started)
1. Mock Framework tutorials:
    * [Mockito (Java)](https://www.tutorialspoint.com/mockito/index.htm)
    * [mock (Python)](https://docs.python.org/3/library/unittest.mock.html)
    * [SinonJs (Javascript)](https://sinonjs.org/) - works with any javascript unit-testing framework
1. [Refactoring.com, Martin Fowler](https://refactoring.com/catalog/) - catalogs a whole array of refactoring principles and tips.
1. [Test-Driven Development: By Example, Kent Beck](https://www.amazon.sg/Test-Driven-Development-Kent-Beck/dp/0321146530/ref=sr_1_1?tag=pbourgau-20&s=books&ie=UTF8&qid=1495080564&sr=1-1&keywords=tdd+by+example) - meant to inspire developers to embrace TDD, this book discusses the crux of TDD along with best practices, techniques and sample projects.


## References

1. [Beck, K. 2000. Extreme Programming Explained.](https://dl.acm.org/doi/book/10.5555/318762)
1. [Agile Data](http://agiledata.org/essays/tdd.html)
1. [Test-Driven Development - Wikipedia](https://en.wikipedia.org/wiki/Test-driven_development)
</div>