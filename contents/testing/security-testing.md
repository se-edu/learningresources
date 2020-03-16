<frontmatter>
  title: Introduction to Security Testing
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Introduction to Security Testing

**Author(s): [Ahmed Bahajjaj](https://github.com/madanalogy)**<br>
Reviewers: [James Pang](https://github.com/jamessspanggg)

<box id="article-toc">

* [Introduction to Security Testing](#introduction-to-security-testing)
    * [What Is Security Testing?‎](#what-is-security-testing)
    * [Why Have Security Testing?](#why-have-security-testing)
    * [Security Testing in Action](#security-testing-in-action)
    * [References‎](#references)
</box>

## What Is Security Testing?

Testing is defined by the Open Web Application Security Project (<tooltip content="OWASP is a worldwide non-profit organization focused on improving the security of software.">__OWASP__</tooltip>) as a process of comparing the state of a system or application against a set of criteria[^1]. Security testing then is a type of software testing where the criteria to be compared against comprise of security requirements.

The OWASP Zed-Attack-Proxy (ZAP) Project defines software security testing as the process of assessing and testing a system to discover security risks and <tooltip content="A vulnerability is a hole or a weakness in a system that allows an attacker to cause harm to the stakeholders of the system.">__vulnerabilities__</tooltip> of the system and its data[^2]. Assessments are defined as the analysis and discovery of vulnerabilities without attempting to actually exploit those vulnerabilities, and testing as the discovery and attempted exploitation of vulnerabilities.

### Types of Security Testing

Security testing is often broken out, somewhat arbitrarily, according to either the type of vulnerability being tested or the type of testing being done. A common breakout as defined by OWASP ZAP is as follows[^2]:

- Vulnerability Assessment: The system is scanned and analyzed for security vulnerabilities.
- Penetration Testing: The system undergoes analysis and attack from simulated malicious attackers.
- Runtime Testing: The system undergoes analysis and security testing from an end-user.
- Code Review: The system code undergoes a detailed review and analysis looking specifically for security vulnerabilities.

<box id="info">:fas-info-circle: Note that risk assessment, which is commonly listed as part of security testing, is not included in this list. That is because a risk assessment is not actually a test but rather the analysis of the perceived severity of different risks (software security, personnel security, hardware security, etc.) and any mitigation steps for those risks.</box>

## Why Have Security Testing?

Security testing ensures that a system is secure. Since security requirements differ depending on the goals of the project, it is often hard to define the concept of what it means to be secure. One definition is as follows: “Security means that authorized access is granted to protected data and unauthorized access is restricted”[^3]. This definition encompasses two major aspects; first is the protection of data and the second is access to that data. Whether the system is desktop or web-based, we will loosely define security as revolving around the two aforementioned aspects.

Due to the logical limitations of security testing however, passing security testing is not an indication that no flaws exist or that the system adequately satisfies the security requirements[^4]. Why have security testing then? In brief, a system would be more secure with security testing than without.

<box id="info">:fas-info-circle: There is more to the concept of security than the working definition used here. If you would like to learn more about security in-depth, please see the [references](#references).</box>

## Security Testing in Action

The consensus is that it is much more costly to include security testing after software implementation or deployment[^5]. It is necessary then to involve security testing in all phases of a Software Development Life Cycle ([SDLC](https://se-education.org/se-book/processModels/)). A conceptual example of such an involvement is as follows (taken from [Guru99.com](https://www.guru99.com/what-is-security-testing.html#3)):

<center>

<pic src="security-testing/Sec-Test-Fig-1.png" alt="Example">
Figure 1 - Example of Security Testing during a SDLC.
</pic>
</center>

<br>

To understand this in more detail, we can deconstruct the application of security testing into its manifestation in different stages of software development. The following series of steps taken from the OWASP Testing Guide[^1] provide general applications of security testing in a generic SDLC.

### Step 1: Before Development

#### 1.1 Define an SDLC

Before application development starts an adequate SDLC must be defined where security is inherent at each stage. 

#### 1.2 Review Policies and Standards

Ensure that there are appropriate policies, standards, and documentation in place. Documentation is extremely important as it gives development teams guidelines and policies that they can follow. An example of one such documentation is the OWASP Secure Coding Practices Quick Refernce Guide[^6].

People can only do the right thing if they know what the right thing is.

If the application is to be developed in Java, it is essential that there is a Java secure coding standard. If the application is to use cryptography, it is essential that there is a cryptography standard. No policies or standards can cover every situation that the development team will face. By documenting the common and predictable issues, there will be fewer decisions that need to be made during the development process.

### Step 2: During Definition and Design

#### 2.1 Review Security Requirements

Security requirements define how an application works from a security perspective. It is essential that the security requirements are tested. Testing in this case means testing the assumptions that are made in the requirements and testing to see if there are gaps in the requirements definitions.

For example, if there is a security requirement that states that users must be registered before they can get access to a particular section of a website, does this mean that the user must be registered with the system or should the user be authenticated? Ensure that requirements are as unambiguous as possible.

#### 2.1 Review Design & Architecture

Applications should have a documented design and architecture. This documentation can include models, textual documents, and other similar artifacts. It is essential to test these artifacts to ensure that the design and architecture enforce the appropriate level of security as defined in the requirements.

Identifying security flaws in the design phase is not only one of the most cost-efficient places to identify flaws, but can be one of the most effective places to make changes. For example, if it is identified that the design calls for authorization decisions to be made in multiple places, it may be appropriate to consider a central authorization component. If the application is performing data validation at multiple places, it may be appropriate to develop a central validation framework (ie, fixing input validation in one place, rather than in hundreds of places, is far cheaper).

### Step 3: During Development

#### 3.1 Unit & System Tests

System and unit tests should be written to explicitly ensure adherence to functional security requirements. For example, an application with an access control feature should include unit tests to ensure that unauthorised access credentials are not accepted.

<box id="info">:fas-info-circle: If you would like to learn more about writing tests, you can read more [here](https://se-education.org/se-book/testing/).</box>

#### 3.2 Code Walk Through

The security team should perform a code walk through with the developers, and in some cases, the system architects. A code walk through is a high-level walk through of the code where the developers can explain the logic and flow of the implemented code. It allows the code review team to obtain a general understanding of the code, and allows the developers to explain why certain things were developed the way they were.

The purpose is not to perform a code review, but to understand at a high level the flow, the layout, and the structure of the code that makes up the application.

#### 3.3 Code Reviews

Armed with a good understanding of how the code is structured and why certain things were coded the way they were, the tester can now examine the actual code for security defects.

### Step 4: During Deployment‎

#### 4.1 Penetration Testing

Having tested the requirements, analyzed the design, and performed code review, it might be assumed that all issues have been caught. Hopefully this is the case, but penetration testing the application after it has been deployed provides a last check to ensure that nothing has been missed. 

ZAP provides a beginner friendly introduction to penetration testing with a focus on web applications[^2].

### At A Glance

The following is the complete OWASP Testing Framework Workflow taken from the OWASP Testing Guide[^1]:

<center>

<pic src="security-testing/Sec-Test-Fig-2.png" alt="Workflow">
Figure 2 - OWASP Testing Framework Workflow.
</pic>
</center>

<br>
<br>

## References
[^1]: [OWASP Testing Guide](https://www.owasp.org/images/1/19/OTGv4.pdf): An extremely comprehensive guide on security testing with in-depth coverage of web-application security testing.
[^2]: [OWASP Zed-Attack-Proxy](https://www.zaproxy.org/getting-started/): A tool for conducting vulnerability assessments and penetration testing.
[^3]: [Breakdown by SoftwareTestingHelp.com](https://www.softwaretestinghelp.com/how-to-test-application-security-web-and-desktop-application-security-testing-techniques/): A breakdown of security testing into common attack vectors and recommended tools.
[^4]: [Wikipedia Article on Security Testing](https://en.wikipedia.org/wiki/Security_testing): Coverage on the abstract concepts of security testing and the taxonomy involved.
[^5]: [Overview by Guru99.com](https://www.guru99.com/what-is-security-testing.html): A brief summary of Security Testing and it's role in Software Testing.
[^6]: [OWASP Secure Coding Practices](https://www.owasp.org/images/0/08/OWASP_SCP_Quick_Reference_Guide_v2.pdf): A quick reference guide for secure coding practices.

</div>
