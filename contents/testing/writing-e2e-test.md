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


Integration testing is a part of software testing where individual units of your application  are combined and tested as a group. This usually happens after (unit testing stage)[http://softwaretestingfundamentals.com/unit-testing/] and before the (system testing stage)[https://www.guru99.com/system-testing.html].

## Why is integration testing important to SE?

In a software application, each software module is usually designed and unit tested by different programmers. Hence with different understanding of the software requirements, integration testing is necessary to ensure that software modules work in unity and expose any faults in the interaction between different parts of the units. 

[code block] Suppose you are in charge of the data collection component and it passed the data collected from the users to the data analysis component. If the data analysis component assumed your data package to be of maximum 100 lines in type A but the data you sent has no limit on the number of lines in type B, it is then the integration test that is supposed to find this discrepancy.

Hence, integration testing is an important aspect which can be applied to any software such as web, mobile and desktop applications. I will be mainly focusing on integration testing in terms of the web application. Similar concepts can be adopted by other applications.

More information on instances where integration testing is important can be found on (this article)[https://www.guru99.com/integration-testing.html].

## How does integration work?

## How to create E2E test cases

## Tools to get started with integration testing

## Concluding Remarks

## Useful Resources

</div>

