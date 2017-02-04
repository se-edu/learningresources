## Static Analysis
Static analysis is the process of analysing computer programme **without** executing the problem. This practice is often used to ensure the codes follow certain structures or standards (e.g coding standards).

It is possible to do static analysis manually, but there are automated tools(static analysers) that can assist programme and developer in this process. 

### Why static analysis?

#### Analyse thoroughly
In some situations, it is impossible to write all test cases to achieve 100% test coverage. There will be some places in the code that are not covered by test cases, which may result in bugs. In static analysis, all the related file/code will be analysed.

#### Enforce a standard in project
Many projects follow a certain coding standard. For example, some project require the following format for if statement.

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
Such standard will be forced in static analysis.

#### Find the potential bugs early
Static analysis can find bugs before the execution. For example, some programmers may forget to add `break` statement in `switch` statement.

``` java
switch(color) {
case 'blue':
	value = 1;
case 'green':
	value = 2;
}
```
Static analysis tools will automatically alert the programme about the potential bugs.


### Limitation of static analysis

#### False positives
If static analysis tools are used in static analysis, as the tools only recognised pattern, false positive may be introduced.

``` java
try {
	// logc part
} catch (Throwable t) {
	// alert to user
}
```
For example, the above code will catch any throwable object and alert user that a fatal error has occured in the system. In many static analysis tools, catching `Throwable` is regarding as a bad practice and thus will prompt the error to user. In this case, it is acceptable to catch `Throwable` and the error is a false positive.

Many static analysis tools will provide way to suppress the warning. For example `@SuppressWarnings("PMD.AvoidCatchingThrowable") // used as fallback` can be used to suppress the warning.

#### Cannot catch error introducted in runtime envirnment
Since static analysis is done without executing the programme. Some vulnerabilities that introduced in the runtime cannot be caught.

### How to do static Analysis (Static Analysis Tools)
The are several static analysis tools that can be used to assist the process. The detailed discussion of them is as below:

- [CheckStyle](checkStyle.md)
- PMD
- findBugs
- eslint

### References

- [Why Static Code Analysis is Important?](http://javarevisited.blogspot.sg/2014/02/why-static-code-analysis-is-important.html)
