## Static Analysis
Static analysis is the process of analysing computer programme **without** executing the code. This practice is often used to ensure that codes follow certain structures or standards (e.g coding standards).

It is possible to do static analysis manually, but there are automated tools(static analysers) that can assist developers in this process. 

### Why Static Analysis?

#### Analyse Thoroughly
In some situations, it is impossible to achieve 100% test coverage. There will be some sections in the code that are not covered by test cases, which may result in bugs ([see](#find-potential-bugs-early) how static analysis will help you find bugs). In static analysis, all the related files/codes will be analysed.

#### Find Potential Bugs Early
Static analysis can find bugs before the execution. For example, some programmers may forget to add `break` statement in `switch` statement.

``` java
switch(colour) {
case 'blue':
	value = 1;
case 'green':
	value = 2;
}
```
Static analysis tools will automatically alert the programmers about the potential problems/bugs.

#### Enforce a Standard in Project
Many projects enforce certain coding standards. For example, some project require the following format for `if` statement.

``` java
if (condition) {
	// true
} else {
	// false
}
``` 

While others enforce the following standard:

``` java
if (condition) {
	// true
}
else {
	// false
}
```
Such standards can be configured in static analysis tools and the tools will help you enforce the standards.

#### Improve Code Quality
Static analysis will pick up common pitfalls in coding and suggest changes to help you improve your code quality. For example, for the following Java code:

``` java
if (isConditionTrue()) {
	return true;
} else {
	return false;
}
```
Majority of static analysis tools will point out that this can be simplified to:

``` java
return isConditionTrue();
```

### Limitation of Static Analysis

#### False positives
Since static analysis tools only recognize patterns, there might be false positives introduced.

``` java
try {
	// logic part
} catch (Throwable t) {
	// alert user
}
```
For example, the above code will catch any `Throwable` object and alert users that a fatal error has occurred in the system. In many static analysis tools, catching `Throwable` is regarded as a bad practice and thus the tools will prompt the error to developers. However, in this case, we want to provide a friendly alert for system crash instead of showing an ugly stack track. It is acceptable to catch `Throwable` and thus the violation detected by static analysis tools is a false positive.

**Solution**: Many static analysis tools provide ways to suppress the warnings. For example, in [PMD](PMD.md) (a static analysis tool), `@SuppressWarnings` annotation can be used. In this case, 

``` java 
@SuppressWarnings("PMD.AvoidCatchingThrowable") // used as fallback
```
is the correct way to suppress warnings.

#### Cannot Catch Error Introduced in Runtime Environment
Since static analysis is done without executing the programme. Some vulnerabilities that are introduced in the runtime cannot be caught. Thus, you should **not** merely depend on static analysis tools to find bugs. Comprehensive test cases are also needed to verify the functionalities in logic, UI and etc.

### How to Do Static Analysis (Static Analysis Tools)
There are several static analysis tools that can be used to assist the process.

- [List of tools for static code analysis - Wikipedia](https://en.wikipedia.org/wiki/List_of_tools_for_static_code_analysis)
- [Codacy - Automated code reviews & code analytics](https://www.codacy.com/)
- [Review of Java Static Analysis Tools](https://blog.codacy.com/review-of-java-static-analysis-tools-5ad86cfc8ae2#.af3t0ev32)

Here, we will discuss several static analysis tools in detail.

- [CheckStyle](checkStyle.md) (for Java only)
- [PMD](PMD.md)
- findBugs
- eslint

### References

- [Why Static Code Analysis is Important?](http://javarevisited.blogspot.sg/2014/02/why-static-code-analysis-is-important.html)
