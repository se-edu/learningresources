<frontmatter>
  title: Profiling a Desktop Application In Visual Studio 2015
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Profiling a Desktop Application In Visual Studio 2015

Author: [Ong Heng Le](https://github.com/initialshl)

## Overview

This tutorial demonstrates how to get an overview of your desktop application's performance
using the profiler included in [Visual Studio 2015](https://www.visualstudio.com/downloads/). 

## 1. Run a performance profiling session

To start a performance profiling session, follow this guide on 
[Creating and running a performance session](https://msdn.microsoft.com/en-us/library/ms182372.aspx#Anchor_0).

## 2. View the performance report

The profiler will generate the performance report after the profiling session, which you 
can explore on your own. *Inclusive and exclusive data* provides meaningful information 
about the execution time of each function.

* **Inclusive samples**: Collected during execution of the function itself and all functions it calls
* **Exclusive samples**: Collected during execution of the function itself only

> The Visual Studio Profiler Team Blog has a [good explanation on inclusive and exclusive data values](https://blogs.msdn.microsoft.com/profiler/2004/06/09/what-are-exclusive-and-inclusive/).

## 3. Trace down to the problem

You can identify the performance issues using the performance report. The `Summary` view 
shows these two useful information analyzed by the profiler.

* **Functions With Most Individual Work**: The functions which took up the most execution time *(exclusive samples)*
* **Hot Path**: The branch of the call tree which took up the most execution time *(inclusive samples)*

To locate performance issues quickly, the **Functions With Most Individual Work** provides 
a list of functions which are usually candidates for optimization.

If you would like to trace down to the problem more carefully, the **Hot Path** is a good 
starting point. This will familiarize you about the most expensive execution path taken 
by your program. 

> You can follow this [walkthrough](https://msdn.microsoft.com/en-us/library/ms182398.aspx)
to experience how to identify performance problems and optimize your code.

## 4. Identify areas for improvements upwards

You have identified the problem, and may now want to optimize the code in the 
function body. But before that, here's a final tip: It is sometimes easier 
(and better) to optimize by 
**reducing the number of calls to that function in its calling functions.** 

## Resources

1. The general steps in profiling your program: [Beginners Guide to Performance Profiling](https://msdn.microsoft.com/en-us/library/ms182372.aspx)
1. Read my chapter on performance profiling for more advanced topics: [Performance Profiling](PerformanceProfiling.md)

</div>
