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
Reviewers: [James Pang](https://github.com/jamessspanggg), [Jeffry Lum](https://github.com/j-lum)

<box id="article-toc">

* [Introduction to Security Testing](#introduction-to-security-testing)
    * [What Is Security Testing?‎](#what-is-security-testing)
    * [Why Have Security Testing?](#why-have-security-testing)
    * [Security Testing in Action](#security-testing-in-action)
    * [Where to go from here?](#where-to-go-from-here)
</box>

## What Is Security Testing?

Testing is defined by the Open Web Application Security Project (<tooltip content="OWASP is a worldwide non-profit organization focused on improving the security of software.">__OWASP__</tooltip>) as a process of comparing the state of a system or application against a set of criteria[^1]. Security testing then is a type of software testing where the criteria to be compared against comprise of security requirements.

The OWASP Zed-Attack-Proxy (ZAP) Project defines software security testing as the process of assessing and testing a system to discover security risks and <tooltip content="A vulnerability is a hole or a weakness in a system that allows an attacker to cause harm to the stakeholders of the system.">__vulnerabilities__</tooltip> of the system and its data[^2]. Assessments are defined as the analysis and discovery of vulnerabilities without attempting to actually exploit those vulnerabilities, and testing as the discovery and attempted exploitation of vulnerabilities.

### Types of Security Testing

Security testing is often broken out, somewhat arbitrarily, according to either the type of vulnerability being tested or the type of testing being done. A common breakout as defined by OWASP ZAP is as follows[^2]:

- __Vulnerability Assessment__: The system is scanned and analyzed for security vulnerabilities.
- __Penetration Testing__: The system undergoes analysis and attack from simulated malicious attackers.
- __Runtime Testing__: The system undergoes analysis and security testing from an end-user.
- __Code Review__: The system code undergoes a detailed review and analysis looking specifically for security vulnerabilities.

<box type="info">Note that risk assessment, which is commonly listed as part of security testing, is not included in this list. That is because a risk assessment is not actually a test but rather the analysis of the perceived severity of different risks (software security, personnel security, hardware security, etc.) and any mitigation steps for those risks.</box>

## Why Have Security Testing?

Security testing ensures that a system is secure. Since security requirements differ depending on the goals of the project, it is often hard to define the concept of what it means to be secure. One definition is as follows[^3]: <box type="definition">Security means that authorized access is granted to protected data and unauthorized access is restricted.</box> This definition encompasses two major aspects; first is the protection of data and the second is access to that data. Whether the system is desktop or web-based, we will loosely define security as revolving around the two aforementioned aspects.

Due to the logical limitations of security testing however, passing security testing is not an indication that no flaws exist or that the system adequately satisfies the security requirements[^4]. Why have security testing then? In brief, a system would be more secure with security testing than without.

<box type="warning">There is more to the concept of security than the working definition used here.</box>

## Security Testing in Action

The consensus is that it is much more costly to include security testing after software implementation or deployment[^5]. It is necessary then to involve security testing in all phases of a Software Development Life Cycle [(SDLC)](https://se-education.org/se-book/processModels/). A conceptual example of such an involvement is as follows (taken from [Guru99.com](https://www.guru99.com/what-is-security-testing.html#3)):

<center>
<pic src="security-testing/Sec-Test-Fig-1.png" width="800" alt="Example">
Figure 1 - Example of Security Testing during a SDLC.
</pic>
</center>

<br>

To understand this in more detail, we can deconstruct the application of security testing into its manifestation in different stages of software development.

### At a Glance

<center>
<pic src="security-testing/Sec-Test-Fig-2.png" width="650" alt="Workflow">
Figure 2 - Testing Framework Workflow from the OWASP Testing Guide.
</pic>
</center>

<br>

The following series of steps adapted from the OWASP Testing Guide[^1] provide general applications of security testing in a generic SDLC process.

### Step 1: Before Development

#### 1.1 Define an SDLC

Before application development starts, an adequate SDLC must be defined where security is inherent at each stage. 

#### 1.2 Review Policies and Standards

Ensure that there are appropriate policies, standards, and documentation in place. Documentation is extremely important as it gives development teams guidelines and policies that they can follow. An example of one such documentation is the OWASP Secure Coding Practices Quick Refernce Guide[^6].

<box type="important">People can only do the right thing if they know what the right thing is.</box>

If the application is to be developed in Java, it is essential that there is a Java secure coding standard. If the application is to use cryptography, it is essential that there is a cryptography standard. No policies or standards can cover every situation that the development team will face. By documenting the common and predictable issues, there will be fewer decisions that need to be made during the development process.

### Step 2: During Definition and Design

#### 2.1 Review Security Requirements

Security requirements define how an application works from a security perspective. It is essential that the security requirements are tested. In particular, assumptions that are made in the requirements are tested while checking if there are any gaps in the requirement definitions.

For example, if there is a security requirement that states that users must be registered before they can get access to a particular section of a website, does this mean that the user must be registered with the system or should the user be authenticated? Ensure that requirements are as unambiguous as possible.

#### 2.2 Review Design & Architecture

Applications should have a documented design and architecture. This documentation can include models, textual documents, and other similar artifacts. It is essential to test these artifacts to ensure that the design and architecture enforce the appropriate level of security as defined in the requirements.

Identifying security flaws in the design phase is not only one of the most cost-efficient places to identify flaws, but can be one of the most effective places to make changes. For example, if it is identified that the design calls for authorization decisions to be made in multiple places, it may be appropriate to consider having a central component responsible for authorization. If the application is performing data validation at multiple places, it may be appropriate to develop a central validation framework (i.e. fixing input validation in one place, rather than in hundreds of places).

### Step 3: During Development

#### 3.1 Unit & System Tests

[Unit tests](https://se-education.org/se-book/testing/#unit-testing) and [system tests](https://se-education.org/se-book/testing/#system-testing) should be written to explicitly ensure adherence to functional security requirements. For example, an application with an access control feature should include unit tests to ensure that unauthorised access credentials are not accepted.

#### 3.2 Code Walk Through

The security team should perform a code walk through with the developers, and in some cases, the system architects. A code walk through is a high-level walk through of the code where the developers can explain the logic and flow of the implemented code. It allows the code review team to obtain a general understanding of the code, and allows the developers to explain why certain things were developed the way they were.

The purpose is not to perform a code review, but to understand at a high level: the flow, the layout, and the structure of the code that makes up the application.

#### 3.3 Code Reviews

Armed with a good understanding of how the code is structured and why certain things were coded the way they were, the tester can now examine the actual code for security defects.

### Step 4: During Deployment‎

#### 4.1 Penetration Testing

Having tested the requirements, analyzed the design, and performed code review, it might be assumed that all issues have been caught. Hopefully this is the case, but penetration testing the application after it has been deployed provides a last check to ensure that nothing has been missed. 

<box type="tip">[ZAP](https://www.zaproxy.org) provides a beginner friendly introduction to penetration testing with a focus on web applications.</box>

## Where to Go From Here?

If you would like a more in-depth understanding of security testing, please read the OWASP Testing Guide[^1]. Now that you know conceptually what security testing is about and how it's generally applied, you can afford to focus on topics depending on your role and interests:

- __Developer__: Focus on how you can translate security requirements into actual code. Additionally, it would be worth your while as well to go through the OWASP Secure Coding Practices Quick Reference Guide[^6].
- __Project Manager__: Your role is crucial before deployment (in selecting an SDLC and setting out project policies) as well as during requirements definition. It is recommended to read entirely the OWASP Testing Guide[^1] to have a more thorough understanding of what is required.
- __Security Specialist__: Your duties often come in the requirements definition stage and system deployment stage, maybe during code walk through as well depending on organisational decisions. Start with the Wikipedia Article on Security Testing[^4] to learn more about security concepts and OWASP ZAP[^2] to learn basic penetration testing.

[^1]: [OWASP Testing Guide](https://www.owasp.org/images/1/19/OTGv4.pdf): An extremely comprehensive guide on security testing with in-depth coverage of web-application security testing.
[^2]: [OWASP Zed-Attack-Proxy](https://www.zaproxy.org/getting-started/): A tool for conducting vulnerability assessments and penetration testing.
[^3]: [Breakdown by SoftwareTestingHelp.com](https://www.softwaretestinghelp.com/how-to-test-application-security-web-and-desktop-application-security-testing-techniques/): A breakdown of security testing into common attack vectors and recommended tools.
[^4]: [Wikipedia Article on Security Testing](https://en.wikipedia.org/wiki/Security_testing): Coverage on the abstract concepts of security testing and the taxonomy involved.
[^5]: [Overview by Guru99.com](https://www.guru99.com/what-is-security-testing.html): A brief summary of Security Testing and it's role in Software Testing.
[^6]: [OWASP Secure Coding Practices](https://www.owasp.org/images/0/08/OWASP_SCP_Quick_Reference_Guide_v2.pdf): A quick reference guide for secure coding practices.

</div>
