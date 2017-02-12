# Performance Profiling

Author: [Ong Heng Le](https://github.com/initialshl)

<br>

# Overview

Software profiling is collecting data about a program's execution, by sampling statistical 
data during the program's execution or by modifying the program itself. Profilers may 
collect data such as function timings, memory allocation and CPU usage in approximated 
or exact details. 

Performance profiling mostly involves sampling the CPU and collecting function timings 
to identify portions in the program which takes up a significant amount of execution time. 
These can be used to identify potential areas for code optimization.

### Resources:
1. [Profiling Overview](https://msdn.microsoft.com/en-us/library/bb384493(v=vs.110).aspx)

<br>

# Performance Profiling For The First Time

Most profilers are similar in their functionalities, user interface, and use of 
technical terms. You are encouraged to try this tutorial using your preferred profiling 
tools on your own project, while making neccessary adjustments when required. <br>

> This tutorial demonstrates using the profiler included in [Visual Studio 2015](https://www.visualstudio.com/downloads/).

<br>

## 1. Running a performance profiling session

With your project opened in Visual Studio 2015, run the profiler found in the menu under 
`Debug -> Performance Profiler`. 

> You can follow this guide on [Creating and running a performance session](https://msdn.microsoft.com/en-us/library/ms182372.aspx#Anchor_0).

Before you start, you will be asked to choose a profiling method. CPU sampling and 
instrumentation are the two common types of profiling methods which may be the most 
useful for you.

- | CPU Sampling | Instrumentation
--- | --- | ---
**Description** | Collects *sample counts* for each function in the call stack at fixed CPU intervals | Collects *elapsed timings* for each function
**Recommended for** | Finding potential areas for optimization for the first time | Investigating I/O bottlenecks
**Not suitable for** | Profiling thread-blocking operations (such as I/O requests) as no information can be collected in such situations | Profiling entire large projects due to its large overhead and file size of the generated report

Generally, you should use **CPU sampling** to get an overview of performance issues in your 
application, and use **instrumentation** to profile specific portions of your code when needed.
> Read more about the [differences](https://blogs.msdn.microsoft.com/ejarvi/2005/04/07/the-choice-between-sampling-and-instrumentation/) between 
[CPU sampling](https://msdn.microsoft.com/en-us/library/dd264994.aspx#Anchor_0) and 
[instrumentation](https://msdn.microsoft.com/en-us/library/dd264994.aspx#Anchor_1).

If you are still unsure of which profiling method to use, choose **CPU sampling** as it is 
suitable for most general uses.

<br>

## 2. Viewing the performance report

After your profiling session has ended, the profiler will analyze the data collected and 
generate a comprehensive performance report, which you can explore on your own. You may 
need to know the differences between the technical terms *inclusive and exclusive 
data values*.

* **Inclusive samples (or elapsed time)**: Collected during execution of the function itself and all functions it calls
* **Exclusive samples (or elapsed time)**: Collected during execution of the function itself only

> The Visual Studio Profiler Team Blog has a [good explanation on inclusive and exclusive data values](https://blogs.msdn.microsoft.com/profiler/2004/06/09/what-are-exclusive-and-inclusive/).

<br>

## 3. Dig down to the problem

We now start to identify the performance issues from the performance report. The two 
useful information analyzed by the profiler are shown in the `Summary` view.

* **Hot Path**: The branch of the call tree which took up the most execution time *(inclusive samples)*
* **Functions With Most Individual Work**: The functions which took up the most execution time *(exclusive samples)*

To locate performance issues quickly, the **Functions With Most Individual Work** provides 
a list of functions which are usually good candidates for optimization.

If you would like to trace down to the problem more carefully, the **Hot Path** is a good 
starting point. 

1. Start by clicking on a function with high *inclusive samples*. 
1. In the `Function Details` view, compare the samples in the Function body and the Called functions.
1. If a Called function has more samples, click on it to go to its `Function Details` view.
1. Repeat steps 2 and 3 until you reach a Function body with higher samples than the Called functions.

> You can follow this [walkthrough](https://msdn.microsoft.com/en-us/library/ms182398.aspx)
to get a good practice on identifying performance problems.

<br>

## 4. Identify areas for improvements upwards

Now that you have identified the problem, you can now start to optimize the code in the 
function body. But before that, here's a final tip: it is sometimes possible (and easier) 
to optimize by reducing the number of calls to that function from its calling functions.

1. In the `Function Details` view, click on the Calling functions with high *inclusive samples*.
1. Examine the code in the Function Code View and reduce the number of function calls if possible.
1. You may repeat step 2 to investigate further upwards, or return to the function under optimization.

<br>

## Resources:
1. [Beginners Guide to Performance Profiling](https://msdn.microsoft.com/en-us/library/ms182372.aspx)
1. [Understanding Performance Collection Methods](https://msdn.microsoft.com/en-us/library/dd264994.aspx)
1. [The Choice Between Sampling and Instrumentation](https://blogs.msdn.microsoft.com/ejarvi/2005/04/07/the-choice-between-sampling-and-instrumentation/)
1. [What are Exclusive and Inclusive?!](https://blogs.msdn.microsoft.com/profiler/2004/06/09/what-are-exclusive-and-inclusive/)
1. [Walkthrough: Identifying Performance Problems](https://msdn.microsoft.com/en-us/library/ms182398.aspx)

<br>

# Advanced Topics

## Sampling Interval

CPU sampling, by default in Visual Studio 2015, captures data at an interval of 
10,000,000 CPU cycles. You can set a custom interval or do sampling based on events, 
such as page faults or system calls, in the sampling settings.

> Read more about [other types of sampling events and how to choose sampling events](https://msdn.microsoft.com/en-us/library/ms182376.aspx).

### Resources:
1. [How to: Choose Sampling Events](https://msdn.microsoft.com/en-us/library/ms182376.aspx)
1. [YourKit Java Profiler Help - CPU sampling settings](https://www.yourkit.com/docs/java/help/sampling_settings.jsp)

<br>

## Instrumentation Overhead

Instrumentation profiling incurs a substantial overhead, which is an increase in size 
and execution time of the program under profiling. Large projects may be experience a 
slow down during instrumentation profiling, and the generated reports may reach gigabytes 
in file sizes.

> In such cases, it is recommended to [limit instrumentation to specific functions](https://msdn.microsoft.com/en-us/library/cc470663.aspx), 
instead of using instrumentation on the entire project.

Fortunately, most profilers try to reduce the instrumentation overhead and the file size 
of the generated report. The profilers may exclude *small functions*, which are short 
functions that do not make any function calls, and treat the time as being spent in their 
calling functions.

However, this default behavior may sometimes be undesirable when you want to examine 
*small functions* carefully.

> You can set your profiler to [include/exclude *small functions* from instrumentation](https://msdn.microsoft.com/en-us/library/bb514150.aspx).

### Resources:
1. [How to: Limit Instrumentation to Specific Functions](https://msdn.microsoft.com/en-us/library/cc470663.aspx)
1. [How to: Limit Instrumentation to Specific DLLs](https://msdn.microsoft.com/en-us/library/bb385752.aspx)
1. [How to: Exclude or Include Short Functions from Instrumentation](https://msdn.microsoft.com/en-us/library/bb514150.aspx)
1. [Excluding Small Functions From Instrumentation](https://blogs.msdn.microsoft.com/profiler/2008/07/08/excluding-small-functions-from-instrumentation/)

<br>

# Profiling Other Types Of Data

Other than collecting performance statistics and timing data, the profiler is also able 
to collect other data such as memory allocation and GPU usage. 

> Here is the list of [profiling tools](https://msdn.microsoft.com/en-us/library/mt210448.aspx) available in Visual Studio 2015.

Here are some other resources which may interest you.

1. [How to: Collect Performance Data for a Web Site](https://msdn.microsoft.com/en-us/library/2s0xxa1d.aspx)
1. [Collecting .NET Memory Allocation and Lifetime Data](https://msdn.microsoft.com/en-us/library/dd264934.aspx)
1. [Collecting Thread and Process Concurrency Data](https://msdn.microsoft.com/en-us/library/dd265004.aspx)
1. [Collecting tier interaction data](https://msdn.microsoft.com/en-us/library/dd465169.aspx)
