<frontmatter>
  title: Performance Profiling
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Performance Profiling

Author: [Ong Heng Le](https://github.com/initialshl)

## Overview

A performance profiler is a tool which collects data (such as function timings) about 
your program to identify the areas with performance issues, commonly used in code 
optimization. There are two common types of performance profiling methods.

* **CPU sampling**: Collects *samples* at fixed intervals, which provides an overview of your program's performance
* **Instrumentation**: Collects detailed *elapsed times* for your program's function calls

Learn [which profiling method to use for your program](https://blogs.msdn.microsoft.com/ejarvi/2005/04/07/the-choice-between-sampling-and-instrumentation/) 
when choosing between CPU sampling and instrumentation.

## Performance Profiling for the First Time

This article provides resources to help you identify the performance issues as you 
perform profiling on your program for the first time. The general steps in profiling a 
program for the first time are:

1. Run a performance profiling session 
1. View the performance report 
1. Trace down to the problem 
1. Identify areas for improvements upwards 

Most profilers are similar in their functionalities, user interface, and use of 
technical terms. You may adapt the tutorials in this article to your preferred profiling 
tools on your own project. 

* [Profiling a Desktop Application In Visual Studio 2015](ProfilingDesktopAppVS2015.html)

## Advanced Topics

### Sampling Interval

CPU sampling captures data at fixed intervals, usually based on CPU cycles or time. Many 
profilers offer the option to **set a custom interval or perform sampling based on events**, 
such as page faults or system calls, in the sampling settings.

* For Visual Studio 2015: [How to: Choose Sampling Events](https://msdn.microsoft.com/en-us/library/ms182376.aspx)<br>
* For YourKit Java Profiler: [YourKit Java Profiler Help - CPU sampling settings](https://www.yourkit.com/docs/java/help/sampling_settings.jsp)

### Instrumentation Overhead

Instrumentation profiling incurs a substantial overhead, which is an increase in file size 
and execution time of the program. This makes it unsuitable for large projects. In such 
cases, it is recommended to **limit instrumentation to a specific portion of your project**. 

* For Visual Studio 2015: [How to: Limit Instrumentation to Specific Functions](https://msdn.microsoft.com/en-us/library/cc470663.aspx) 
or [Specific DLLs](https://msdn.microsoft.com/en-us/library/bb385752.aspx)

To reduce instrumentation overhead, some profilers may exclude *small functions*, which 
are short functions that do not make any function calls, and treat the time as being 
spent in their calling functions. This behavior is usually enabled by default, which may 
be undesirable when you want to **examine *small functions* carefully**. Most profilers 
offer the option to change this behavior.

* For Visual Studio 2015: [How to: Exclude or Include Short Functions from Instrumentation](https://msdn.microsoft.com/en-us/library/bb514150.aspx)

## Profiling Other Types of Data

Other than collecting performance statistics and timing data, profilers are also able 
to collect other information such as memory allocation and GPU usage. 

For Visual Studio 2015: 

1. List of [Profiling Tools](https://msdn.microsoft.com/en-us/library/mt210448.aspx) (A [table](https://msdn.microsoft.com/en-us/library/mt210448.aspx#Anchor_10) which shows the most suitable tool for each project)
1. [How to: Collect Performance Data for a Web Site](https://msdn.microsoft.com/en-us/library/2s0xxa1d.aspx) (ASP.NET and JavaScript)
1. [Collecting .NET Memory Allocation and Lifetime Data](https://msdn.microsoft.com/en-us/library/dd264934.aspx)
1. [Collecting Thread and Process Concurrency Data](https://msdn.microsoft.com/en-us/library/dd265004.aspx)
1. [Collecting tier interaction data](https://msdn.microsoft.com/en-us/library/dd465169.aspx)

## Resources

1. What is a software profiler?: [Profiling Overview](https://msdn.microsoft.com/en-us/library/bb384493(v=vs.110).aspx)
1. Common performance profiling methods: [Understanding Performance Collection Methods](https://msdn.microsoft.com/en-us/library/dd264994.aspx)
1. Learn the best practices in profiling: [Advanced Profiling: Theory in Practice with NetBeans IDE](https://netbeans.org/community/magazine/html/04/profiler.html)
1. Why do profilers exclude small functions from instrumentation by default?: [Excluding Small Functions From Instrumentation](https://blogs.msdn.microsoft.com/profiler/2008/07/08/excluding-small-functions-from-instrumentation/)
</div>
