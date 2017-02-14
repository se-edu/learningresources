## Static Analysis
Static analysis is the process of analysing computer programme **without** executing the problem. This practice is often used to ensure the codes follow certain structures or standards (e.g coding standards).

It is possible to do static analysis manually, but there are automated tools(static analysers) that can assist developers in this process. 

### Why Static Analysis?

#### Analyse Thoroughly
In some situations, it is impossible to write all test cases to achieve 100% test coverage. There will be some places in the code that are not covered by test cases, which may result in bugs. In static analysis, all the related files/codes will be analysed.

#### Enforce a Standard in Project
Many projects enforce certain coding standards. For example, some project require the following format for if statement.

``` java
if (condition) {
	// true
} else {
	// false
}
``` 

While others enforce the following standard

``` java
if (condition) {
	// true
}
else {
	// false
}
```
Such standards can be configured and forced in static analysis.

#### Find Potential Bugs Early
Static analysis can find bugs before the execution. For example, some programmers may forget to add `break` statement in `switch` statement.

``` java
switch(color) {
case 'blue':
	value = 1;
case 'green':
	value = 2;
}
```
Static analysis tools will automatically alert the programmers about the potential problem.

#### Improve Code Quality
Static analysis will pick up common pitfalls in coding and suggest changes to help you improve your code quality. For example, for the following Java code:

``` java
if (isConditionTrue()) {
	return true;
} else {
	return false;
}
```
Majority of static analysis tools will point out that this can be simplified to

``` java
return isConditionTrue();
```

### Limitation of Static Analysis

#### False positives
If static analysis tools are used in static analysis, as the tools only recognised patterns, false positive may be introduced.

``` java
try {
	// logc part
} catch (Throwable t) {
	// alert to user
}
```
For example, the above code will catch any throwable object and alert users that a fatal error has occurred in the system. In many static analysis tools, catching `Throwable` is regarding as a bad practice and thus will prompt the error to users. In this case, it is acceptable to catch `Throwable` and the violation is a false positive.

**Solution**: Many static analysis tools provide ways to suppress the warning. For example, in [PMD](PMD.md), `@SuppressWarnings` annotation can be used. In this case, `@SuppressWarnings("PMD.AvoidCatchingThrowable") // used as fallback` is the correct way to suppress warnings.

#### Cannot Catch Error Introduced in Runtime Environment
Since static analysis is done without executing the programme. Some vulnerabilities that are introduced in the runtime cannot be caught.

### How to Do Static Analysis (Static Analysis Tools)
There are several static analysis tools that can be used to assist the process. The detailed discussion of them is as below:

- [CheckStyle](checkStyle.md) (for Java only)
- [PMD](PMD.md)
- findBugs
- eslint

### References

- [Why Static Code Analysis is Important?](http://javarevisited.blogspot.sg/2014/02/why-static-code-analysis-is-important.html)
