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

### Resources used:
1. What is a software profiler?: [Profiling Overview](https://msdn.microsoft.com/en-us/library/bb384493(v=vs.110).aspx)
1. Common performance profiling methods: [Understanding Performance Collection Methods](https://msdn.microsoft.com/en-us/library/dd264994.aspx)

# Performance Profiling For The First Time

Most profilers are similar in their functionalities, user interface, and use of 
technical terms. 

> This tutorial demonstrates using the profiler included in [Visual Studio 2015](https://www.visualstudio.com/downloads/). 

You are encouraged to adapt this tutorial to your preferred profiling tools on your own 
project. Please read the relevant sections in the user's guide of your profiler if 
you encounter any significant differences in their usage.

## 1. Run a performance profiling session

> To start a performance profiling session in Visual Studio 2015, follow this guide on 
[Creating and running a performance session](https://msdn.microsoft.com/en-us/library/ms182372.aspx#Anchor_0).

## 2. View the performance report

The profiler will generate the performance report after the profiling session, which you 
can explore on your own. *Inclusive and exclusive data* provides meaningful information 
about the execution time of each function.

* **Inclusive samples**: Collected during execution of the function itself and all functions it calls
* **Exclusive samples**: Collected during execution of the function itself only

The Visual Studio Profiler Team Blog has a [good explanation on inclusive and exclusive data values](https://blogs.msdn.microsoft.com/profiler/2004/06/09/what-are-exclusive-and-inclusive/).

## 3. Trace down to the problem

You can identify the performance issues using the performance report. Most performance 
reports show these two useful information analyzed using the collected data.

* **Functions With Most Individual Work**: The functions which took up the most execution time *(exclusive samples)*
* **Hot Path**: The branch of the call tree which took up the most execution time *(inclusive samples)*

To locate performance issues quickly, the **Functions With Most Individual Work** provides 
a list of functions which are usually candidates for optimization.

If you would like to trace down to the problem more carefully, the **Hot Path** is a good 
starting point. This will familiarize you about the most expensive execution path taken 
by your program. 

> If you are using Visual Studio 2015, you can follow this [walkthrough](https://msdn.microsoft.com/en-us/library/ms182398.aspx)
to experience how to identify performance problems and optimize your code.

## 4. Identify areas for improvements upwards

You have identified the problem, and may now want to optimize the code in the 
function body. But before that, here's a final tip: 

* It is sometimes easier (and better) to optimize by **reducing the number of calls to 
that function in its calling functions.** 

### Recommended resources:
1. Learn about the best practices in profiling: [Advanced Profiling: Theory in Practice with NetBeans IDE](https://netbeans.org/community/magazine/html/04/profiler.html)

### Resources used:
1. The general steps to profiling your program: [Beginners Guide to Performance Profiling](https://msdn.microsoft.com/en-us/library/ms182372.aspx)

# Advanced Topics

## Sampling Interval

CPU sampling captures data at fixed intervals, usually based on CPU cycles or time. Many 
profilers offer the option to set a custom interval or perform sampling based on events, 
such as page faults or system calls, in the sampling settings.

> Read more about [other types of sampling events and how to choose sampling events](https://msdn.microsoft.com/en-us/library/ms182376.aspx) in Visual Studio 2015.

### Recommended resources:
1. The sampling settings available in a Java profiler: [YourKit Java Profiler Help - CPU sampling settings](https://www.yourkit.com/docs/java/help/sampling_settings.jsp)

## Instrumentation Overhead

Instrumentation profiling incurs a substantial overhead, which is an increase in file size 
and execution time of the program. This makes it unsuitable for large projects. In such 
cases, it is recommended to limit instrumentation to a specific portion of your project. 

> Visual Studio 2015 profiler is able to limit instrumentation to [specific functions](https://msdn.microsoft.com/en-us/library/cc470663.aspx) 
or [specific DLLs](https://msdn.microsoft.com/en-us/library/bb385752.aspx).

To reduce instrumentation overhead, some profilers may exclude *small functions*, which 
are short functions that do not make any function calls, and treat the time as being 
spent in their calling functions.

This behavior is usually enabled by default, which may be undesirable when you want to 
examine *small functions* carefully. Most profilers offer the option to change this 
behavior.

> See how to set Visual Studio 2015 profiler to [include/exclude small functions from instrumentation](https://msdn.microsoft.com/en-us/library/bb514150.aspx).

### Recommended resources:
1. Why do profilers exclude small functions from instrumentation by default?: [Excluding Small Functions From Instrumentation](https://blogs.msdn.microsoft.com/profiler/2008/07/08/excluding-small-functions-from-instrumentation/)

# Profiling Other Types Of Data

Other than collecting performance statistics and timing data, profilers are also able 
to collect other information such as memory allocation and GPU usage. 

> Here is a list of [profiling tools](https://msdn.microsoft.com/en-us/library/mt210448.aspx) available in Visual Studio 2015.

Learn more about the other types of data which can be collected by profilers:

1. [How to: Collect Performance Data for a Web Site](https://msdn.microsoft.com/en-us/library/2s0xxa1d.aspx) (ASP.NET or JavaScript)
1. [Collecting .NET Memory Allocation and Lifetime Data](https://msdn.microsoft.com/en-us/library/dd264934.aspx)
1. [Collecting Thread and Process Concurrency Data](https://msdn.microsoft.com/en-us/library/dd265004.aspx)
1. [Collecting tier interaction data](https://msdn.microsoft.com/en-us/library/dd465169.aspx)
