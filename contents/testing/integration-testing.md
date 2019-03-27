<frontmatter>
  title: Integration Testing
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Integration Testing

**Author(s): [Jacob Li PengCheng](https://github.com/jacoblipech)**

Reviewers:

## What is integration testing?

Integration testing is a part of software testing where individual parts of your application are combined and tested as a whole. This usually happens after the [<tooltip content="Testing of individual components within a system">unit testing stage</tooltip>](http://softwaretestingfundamentals.com/unit-testing/) and before the [<tooltip content="Evaluation of the software against requirements gathered from users and system specifications">system testing stage</tooltip>](https://www.guru99.com/system-testing.html) and it aims to discover faults related to the interaction of components.

## Why is integration testing important?

In a software application, each software module is usually designed and unit tested by different programmers. Since these programmers may work in isolation and have different understandings of the software requirements, integration testing is necessary to ensure that software modules work in unity and expose any faults in the interaction between different units.

<box>Suppose you are in charge of a **data collection component** that passes the **data** collected from users to a **data analysis component**. If the **data analysis component** assumes your **data** to be of maximum 100 lines but the data you send has 200 lines, it may result in data loss, inaccurate results and even system errors. Hence, it is the integration test that is supposed to find this discrepancy before any serious damage is made.</box>

Hence, integration testing is an important process which should be applied in the development of many softwares such as web, mobile and desktop applications. We will mainly be focusing on integration testing in terms of web applications. Similar concepts can be adopted by other applications.

:information_source: More information on instances where integration testing is important can be found on [this article](https://www.guru99.com/integration-testing.html).

## How does integration testing work?

Imagine that you are working on an online web ordering system and a sample architecture diagram of your application is shown as follows:

![Sample architecture diagram](integration-test/sample-architecture.png "Sample architecture diagram")

Using this specific system as an example, below are a list of things you should follow to execute effective integration testing:

1. Prepare the integration test plan.
1. Design the test scenarios, cases and scripts.
1. Executing the test scripts and report defects if any.
1. Tracking defects and re-testing application.

### 1. Prepare the integration test plan

Before the actual implementation of your integration tests, it is important to decide on the approach used. There are a few different approaches of integration testing in which you can adopt depending on the development progress of your application.

#### [Big bang approach](https://www.tutorialspoint.com/software_testing_dictionary/big_bang_testing.htm): 

This approach involves integrating all the components in your design diagram together and testing everything at once in a complete state. This is convenient but it is difficult to isolate defects and there is a high chance of missing critical underlying defects. Big bang integration testing is usually used for smaller applications with few components. 

An example of how big bang integration testing can be applied to our given example is shown below:

![Big bang integration testing diagram](integration-test/big-bang-integration.png "Big bang integration testing diagram]")

#### [Incremental testing approach](https://www.quora.com/What-is-incremental-testing-in-software):

This approach involves integrating two or more logically related components. The other related components are added and tested for proper functioning. This is repeated until all the components are joined and tested successfully. It is usually preferred with any applications having more than 10 different components.

Incremental integration testing is further split into the 3 approaches shown below:

| | [Top-down approach](https://www.guru99.com/integration-testing.html#9) | [Bottom-up approach](https://www.guru99.com/integration-testing.html#8) | [Hybrid / sandwich approach](https://www.guru99.com/integration-testing.html#10) |
| -- | -- | -- | -- |
| **Description**| High level modules are tested first and then lower level modules until all the modules are tested. | The reverse of top-down approach. | A mix of both top-down bottom-up approaches. |
| <span style="color:red">**Advantages**</span>| Early discovery of high level architecture / design defects | Easier to create test cases bottom up | Beneficial for big project to distribute tasks on testing
| | Main control points of the system are tested early | Critical modules on functionalities ae tested first | Allow top-down and bottom-up approach to run side by side | 
| <span style="color:blue">**Disadvantages**</span> | Significant low level modules are tested late in the cycle | There is no testable working system until the higher level modules are build | It is difficult to test for highly interconnected modules |
| | A [<tooltip content="A program that simulates the behaviours of software components">stub</tooltip>](https://stackoverflow.com/questions/463278/what-is-a-stub) is not perfect to simulate data flow | A [<tooltip content="Module with dummy code to temporalities replace a module">driver</tooltip>](http://www.professionalqa.com/test-driver) test is even harder to write than stub | Higher cost from using both driver and stub. You can understand the difference [here](https://www.quora.com/What-is-the-difference-between-stubs-and-drivers-in-software-testing)

:information_source: A more detailed guide on using specific methods for incremental testing together with examples can be found [here](https://www.softwaretestinghelp.com/incremental-testing/).

### 2. Design the test scenarios, cases and scripts

Before the actual coding is done, a basic test strategy deciding the test cases and test data used should be crafted. This usually involves setting a <span style="color:red">test case ID</span>, <span style="color:red">objective</span>, <span style="color:red">description</span> and <span style="color:red">expected result</span>. Using the example shown above, below shows a sample integration test used for the `login and ordering` modules:

~~~
Test case ID: 1
Objective: Check the link between login and ordering modules
Description: Enter login credentials and click on login button
Expected result: To be directed to order food page based on the login user
~~~

<box type="warning">As integration test cases are <tooltip content="It generally takes longer time due to additional overheads such as waiting for dependent modules to respond">expensive operations</tooltip> compared to unit testing, it should focus mainly on the integration of modules together and not on specific actions within the same module.</box>

:information_source: More details about the ways to structure incremental testing can be found on [this article](https://www.softwaretestinggenius.com/various-approaches-in-integration-testing/)

### 3. Executing test scripts and report defects

Depending on the approach you have chosen for your integration plan and the test cases, the way you execute your code for testing will differ.

A big bang approach usually involves all the modules to be developed before you can start with the integration testing.

As for incremental approach, it is usually conducted simultaneously with the modules' development. Hence, stubs and drivers are used to mimic the modules for writing tests.

In both cases, always ensure that all high prioritzed bugs are fixed and closed before moving on.

:information_source: [This article](https://www.guru99.com/test-environment-software-testing.html) shows more details on how to set up a test environment for better integration testing. 

### 4. Tracking defects and re-testing application

In the event of failing your integration test case, it is important to learn how to track down the [<tooltip content="Incorrect behavior observed from the system">defects</tooltip>](https://qacomplete.com/resources/articles/what-is-a-software-defect/)occured. Thereafter, you should make changes to your application to fix them and re-test your application with integration testing to ensure that the defects are no longer there.

:information_source: [This article](http://www.professionalqa.com/defect-tracking-process) covers more details on how to effectively track down defects in a system and fix them.

## Tips for better integration tests

### :thumbsup: Make sure that each component is unit tested before integration testing
By ensuring that each unit test is completed properly, integration testing will be smoother as we can focus mainly on the flow of data between modules.

### :thumbsup: Prioritize the modules to be tested
Despite the need to cover all areas of integration of the application, it is important to ensure that critical modules needs to be tested first.

### :thumbsup: Automate your tests (strongly encouraged)
As far as possible, automate all your tests, especially when you use the incremental approach, since regression testing is important each time you integrate a unit, and manual regression testing can be inefficient. You can find a list of most commonly used [automation tools](https://medium.com/@briananderson2209/best-automation-testing-tools-for-2018-top-10-reviews-8a4a19f664d2) for integration testing.

### :thumbsup: Ensure that all executed test cases are documented
This helps you to identify errors quickly through an integration test. It also helps to standadize the way integration testing is carried out in your application so that everyone can conform to the given standard.

## Tools to get started with integration testing

Here are some useful tools that you can use for your integration testing:

- [Selenium](https://www.seleniumhq.org/). Selenium is an open source test automation framework focusing on web applications. It supports a wide range of programming languages, cross-browser testing with extensive libraries and the ability to create robust test scripts to handle many scenarios.

- [Robotium](https://github.com/RobotiumTech/robotium). Robotium is an Android test automation framework made to simplify black-box tests. It can handle multiple activities automatically, producing fast and robust tests cases.

- [Gauge](https://gauge.org/). Gauge acts as a plugin which can be incorporated to any language or IDE. It is an lightweight cross-platform test automation tool which makes testing easier to maintain, more readable and flexible.

- [Google EarlGrey](https://google.github.io/EarlGrey/). For non web applications, EarlGrey is a native iOS automation test framework allowing developers to write and maintain clear concise tests. It has a powerful built-in synchornization which allows it to reproduce any UI interactivity and test them.

Note that although there are many integration testing tool available, more research needs to be conducted to ensure the compatibility of the tools with your application.

## Concluding Remarks

Ultimately, as a developer, it is important to recognize the importance of integration testing in your application. To better complete integration testing, follow the integration plan and ensure that all of the interfaces in your application are tested.

## Useful Resources

Here are some resources to help you with integration testing:

- [Dos and donts of integration testing](https://www.fogbugz.com/blog/9-integration-testing-dos-and-donts/). This article shows further advices on specific details to take note when writing your own integration test.

- [Other testing tools to start your integration testing](https://www.softwaretestinghelp.com/integration-testing-tools/). Depending on your application, you can refer to more tools which can assist you in getting started with integration testing.

- [Getting started with Selenium for automated website testing](https://wiki.saucelabs.com/display/DOCS/Getting+Started+with+Selenium+for+Automated+Website+Testing). This article gives an overall guide to integrating selenium to automate integration testing for your web application.

</div>
