# Performance Profiling

Author: [Ong Heng Le](https://github.com/initialshl)

# Overview

Performance profiling involves collecting data (such as function timings) 
about a program to identify areas with performance issues. It is a tool that 
is usually used in code optimization. CPU sampling and instrumentation are 
the two most common types of performance profiling methods.

* **CPU sampling**: Collects *samples* at fixed intervals, which provides an overview of your program's performance
* **Instrumentation**: Collects detailed *elapsed timings* about your program

> If you are interested, here are [more technical details](https://blogs.msdn.microsoft.com/ejarvi/2005/04/07/the-choice-between-sampling-and-instrumentation/) about 
[CPU sampling](https://msdn.microsoft.com/en-us/library/dd264994.aspx#Anchor_0) and 
[instrumentation](https://msdn.microsoft.com/en-us/library/dd264994.aspx#Anchor_1).

### Resources:
1. [Profiling Overview](https://msdn.microsoft.com/en-us/library/bb384493(v=vs.110).aspx)
1. [The Choice Between Sampling and Instrumentation](https://blogs.msdn.microsoft.com/ejarvi/2005/04/07/the-choice-between-sampling-and-instrumentation/)
1. [Understanding Performance Collection Methods](https://msdn.microsoft.com/en-us/library/dd264994.aspx)

# Performance Profiling For The First Time

Most profilers are similar in their functionalities, user interface, and use of 
technical terms. This tutorial demonstrates using the profiler included in 
[Visual Studio 2015](https://www.visualstudio.com/downloads/).

> You are encouraged to adapt this tutorial to your preferred profiling 
tools on your own project. <br>

## 1. Run a performance profiling session

Before you start, you will be asked to choose a profiling method. It is recommended to 
use **CPU sampling** for your first profiling session.

> To start a performance profiling session, follow this guide on 
[Creating and running a performance session](https://msdn.microsoft.com/en-us/library/ms182372.aspx#Anchor_0).

## 2. View the performance report

The profiler will generate the performance report after the profiling session, which you 
can explore on your own. *Inclusive and exclusive data* provides meaningful information 
about the execution time of each function.

* **Inclusive samples (or elapsed time)**: Collected during execution of the function itself and all functions it calls
* **Exclusive samples (or elapsed time)**: Collected during execution of the function itself only

> The Visual Studio Profiler Team Blog has a [good explanation on inclusive and exclusive data values](https://blogs.msdn.microsoft.com/profiler/2004/06/09/what-are-exclusive-and-inclusive/).

## 3. Trace down the problem

We now start to identify the performance issues using the performance report. The two 
useful information analyzed by the profiler are shown in the `Summary` view.

* **Hot Path**: The branch of the call tree which took up the most execution time *(inclusive samples)*
* **Functions With Most Individual Work**: The functions which took up the most execution time *(exclusive samples)*

To locate performance issues quickly, the **Functions With Most Individual Work** provides 
a list of functions which are usually candidates for optimization.

If you would like to trace down to the problem more carefully, the **Hot Path** is a good 
starting point. This will familiarize you about the most expensive execution path taken 
by your program. 

> You can follow this [walkthrough](https://msdn.microsoft.com/en-us/library/ms182398.aspx)
to learn how to identify performance problems.

## 4. Identify areas for improvements upwards

Now that you have identified the problem, you can now start to optimize the code in the 
function body. But before that, here's a final tip: 
> It is sometimes possible (and easier) to optimize by reducing the number of calls to 
that function from its calling functions.

### Resources:
1. [Beginners Guide to Performance Profiling](https://msdn.microsoft.com/en-us/library/ms182372.aspx)
1. [What are Exclusive and Inclusive?!](https://blogs.msdn.microsoft.com/profiler/2004/06/09/what-are-exclusive-and-inclusive/)
1. [Walkthrough: Identifying Performance Problems](https://msdn.microsoft.com/en-us/library/ms182398.aspx)

# Advanced Topics

## Sampling Interval

CPU sampling, by default in Visual Studio 2015, captures data at an interval of 
10,000,000 CPU cycles. You can set a custom interval or perform sampling based on events, 
such as page faults or system calls, in the sampling settings.

> Read more about [other types of sampling events and how to choose sampling events](https://msdn.microsoft.com/en-us/library/ms182376.aspx).

### Resources:
1. [How to: Choose Sampling Events](https://msdn.microsoft.com/en-us/library/ms182376.aspx)
1. [YourKit Java Profiler Help - CPU sampling settings](https://www.yourkit.com/docs/java/help/sampling_settings.jsp)

## Instrumentation Overhead

Instrumentation profiling incurs a substantial overhead, which is an increase in file size 
and execution time of the program, which makes it unsuitable for large projects.

> In such cases, it is recommended to [limit instrumentation to specific functions](https://msdn.microsoft.com/en-us/library/cc470663.aspx), 
instead of using instrumentation on the entire project.

To reduce instrumentation overhead, some profilers may exclude *small functions*, which 
are short functions that do not make any function calls, and treat the time as being 
spent in their calling functions.

This behavior is usually enabled by default, which may be undesirable when you want to 
examine *small functions* carefully.

> You can set your profiler to [include/exclude *small functions* from instrumentation](https://msdn.microsoft.com/en-us/library/bb514150.aspx).

### Resources:
1. [How to: Limit Instrumentation to Specific Functions](https://msdn.microsoft.com/en-us/library/cc470663.aspx)
1. [How to: Limit Instrumentation to Specific DLLs](https://msdn.microsoft.com/en-us/library/bb385752.aspx)
1. [How to: Exclude or Include Short Functions from Instrumentation](https://msdn.microsoft.com/en-us/library/bb514150.aspx)
1. [Excluding Small Functions From Instrumentation](https://blogs.msdn.microsoft.com/profiler/2008/07/08/excluding-small-functions-from-instrumentation/)

# Profiling Other Types Of Data

Other than collecting performance statistics and timing data, the profiler is also able 
to collect other data such as memory allocation and GPU usage. 

> Here is the list of [profiling tools](https://msdn.microsoft.com/en-us/library/mt210448.aspx) available in Visual Studio 2015.

Here are some other resources which may interest you.

1. [How to: Collect Performance Data for a Web Site](https://msdn.microsoft.com/en-us/library/2s0xxa1d.aspx)
1. [Collecting .NET Memory Allocation and Lifetime Data](https://msdn.microsoft.com/en-us/library/dd264934.aspx)
1. [Collecting Thread and Process Concurrency Data](https://msdn.microsoft.com/en-us/library/dd265004.aspx)
1. [Collecting tier interaction data](https://msdn.microsoft.com/en-us/library/dd465169.aspx)
