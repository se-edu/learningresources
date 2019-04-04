<frontmatter>
  title: DevOps
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

# DevOps

Authors: [John Yong](https://github.com/whipermr5)

## What is DevOps?

> &ldquo;DevOps is the practice of operations and development engineers participating together in the entire service lifecycle, from design through the development process to production support.&rdquo; &mdash; [the agile admin](https://theagileadmin.com/what-is-devops/)

The term **DevOps** comes from the amalgamation of two words - **Dev**elopment and **Op**eration**s**. While there are many definitions and opinions out there about what DevOps really is, and no one true answer, most agree that it is the idea of Developers and Operations working together to achieve a goal ([source](https://blog.xebialabs.com/2015/04/23/best-guide-to-getting-started-in-devops/)). This involves a **shift in mindset** away from the traditional view of Development and Operations as two distinct teams with few overlapping responsibilities, to a new mindset where developers and operations engineers communicate, collaborate and share the responsibility of developing and delivering software.

Indeed, some see DevOps as a culture shift that involves more than just developers and operations teams, but also everyone involved in delivering value to end users, including testers and Quality Assurance, Project Management and Human Resources teams ([source](http://blog.nwcadence.com/devops-culture-not-team/)). However, this document will focus more on the development and operations side of DevOps, which is what budding software engineers like yourself need to know.

> Tip: for a comprehensive discussion of what DevOps is and is not, refer to *the agile admin*'s article, [What Is DevOps?](https://theagileadmin.com/what-is-devops/)

## Aspects of DevOps

While DevOps is predominantly an organisational culture and mindset, there are several aspects it covers that you, as a software engineering student, need to be familiar with before entering the industry. These are outlined in the DevOps toolchain ([source](https://en.wikipedia.org/wiki/DevOps#DevOps_toolchain)):

1. Code
2. Build
3. Test
4. Package
5. Release
6. Configure
7. Monitor

These can be roughly summarised into four major aspects:

### Code Development & Review

This includes using version control tools, following a process when developing, frequently merging code with a central branch, and going through a code review process.

#### Relevant Tools

Version Control Software (VCS)

- [Git](https://git-scm.com/)
- [Mercurial](https://www.mercurial-scm.org/)

Web-based Hosted VCS

- [GitHub](https://github.com/)
- [GitLab](https://about.gitlab.com/)
- [BitBucket](https://bitbucket.org/)

### Build & Test Automation

Involves using continuous integration tools that automatically build and test your software upon every push to a code repository (Git, Mercurial, etc.), so you don't have to do it yourself. They can also be configured to run on specific branches or pull requests.

#### Relevant Tools

Local Build Tools
> In addition to automating builds, these also manage dependencies

- [Gradle](https://gradle.org/)
- [Ant](http://ant.apache.org/)
- [Maven](https://maven.apache.org/)

Hosted CI Tools
> These integrate with hosted VCS solutions like GitHub

- [Travis CI](https://travis-ci.org/)
- [Circle CI](https://circleci.com/)
- [Jenkins](https://jenkins.io/) (self-hosted)

### Deployment & Infrastructure

This is about getting your software to run in a live server environment so that it is publicly accessible to end users. It also involves packaging your app such that it can be deployed in different environments.

#### Relevant Tools

Infrastructure Management Tools

- [Vagrant](https://www.vagrantup.com/) - portable virtual development environments
- [Docker](https://www.docker.com/) - deployment of applications inside software containers
- [Ansible](https://www.ansible.com/) - automated software provisioning, configuration management and deployment
- [Puppet](https://puppet.com/) - configure infrastructure with declarative code

Cloud Infrastructure

- [DigitalOcean](https://www.digitalocean.com/) - cloud computing for developers
- [Amazon Web Services](https://aws.amazon.com/) - scalable cloud computing services

### Monitoring

After deployment, monitoring the system for any defects or potential defects is essential to minimise outages (infrastructure monitoring). This aspect also includes application performance management (APM) and log analysis.

#### Relevant Tools

- [Icinga](https://www.icinga.com/) - open source system and network monitoring [[install guide]](https://www.digitalocean.com/community/tutorials/how-to-use-icinga-to-monitor-your-servers-and-services-on-ubuntu-14-04)
- [Monit](https://mmonit.com/monit/) - open source monitoring utility that also does auto recovery of services [[install guide]](https://www.digitalocean.com/community/tutorials/lemp-stack-monitoring-with-monit-on-ubuntu-14-04)
- [ELK (Elasticsearch, Logstash, Kibana)](https://www.elastic.co/products) - analytics tools that work well together [[install guide]](https://www.digitalocean.com/community/tutorials/how-to-install-elasticsearch-logstash-and-kibana-elk-stack-on-ubuntu-14-04)

## Getting Started

As a software engineering student, the best way to get started is to gain some practical experience by trying out the various tools mentioned above. This is a suggested process you can try:

1. Contribute to an open source project on GitHub. Make several pull requests and get used to the flow of code review and frequent integration with the base branch.
2. Integrate build and test automation into an existing project that doesn't already use it (perhaps one of your past programming assignments). Write test cases for the project, put the project on GitHub, integrate Travis CI with it and get Travis to run the tests every time you push a commit.
3. Get familiar with the Ops side of things. Get a DigitalOcean droplet (use free credits from the GitHub education pack) and deploy a web app (any past project that you ran on localhost all the time). Try installing and configuring OpenVPN on the droplet (plus point - you get your own private VPN!)
4. Install a monitoring tool on the same droplet to notify you if the web app or the OpenVPN service goes down. Get it to email/ send you a Telegram message if this happens, and test it by manually stopping the web server/ OpenVPN.
5. Bonus: Try setting up automated deployment so your web app gets updated on the fly when code is pushed to the release branch.

## Useful Links

### Quick References
- [What is DevOps?](https://aws.amazon.com/devops/what-is-devops/) - a concise article by Amazon Web Services
- [The Essential DevOps Terms You Need to Know](https://blog.xebialabs.com/2016/03/21/essential-devops-terms/) - understand the most commonly used terms in the industry

### Further Reading
- [Best Guide to Getting Started In DevOps](https://blog.xebialabs.com/2015/04/23/best-guide-to-getting-started-in-devops/) - recommends various places you can read up more on the topic
- [9½ Simple Steps On How To Start With DevOps Today](https://devops.com/9%C2%BD-simple-steps-start-devops-today/) - includes practical suggestions like using a code static analysis tool
- [A Pragmatic Guide to Getting Started with DevOps](http://www.ca.com/us/lpg/~/media/Files/eBooks/a-pragmatic-guide-to-getting-started-with-devops.pdf) - more of a management point of view but interesting
- [The DevOps Handbook: How to Create World-Class Speed, Reliability, and Security in Technology Organizations](https://books.google.com.sg/books/about/The_Devops_Handbook.html?id=XrQcrgEACAAJ) - about managing tech organisations, written by a CTO
</div>
