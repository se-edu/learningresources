# Introduction to Go

Author(s): [Cara Leong](https://github.com/craaaa)

Go (also known as `golang`) is a compiled, statically-typed, garbage-collected language that has special memory safety and concurrent programming features. Born out of frustration with the available languages (e.g. C, C++, Java) and environments for systems programming, Go was [conceptualized by programmers at Google](https://talks.golang.org/2012/splash.article) who sought to create a single language that was efficient to write, build and execute. Go also supports newer developments in computing such as multicore processors and network systems.

Go is an [open source project](https://github.com/golang/go). Its source code may be useful reading for those interested in learning good practices, or simply to find out more about how the language was implemented.

## Getting started

Go provides its own [installation guide](https://golang.org/doc/install) and an interactive [ tour of Go](https://tour.golang.org/). These are useful and highly comprehensive resources for programmers looking to learn the syntax and style of Go. For those who prefer to read existing code examples, [Go by Example](https://gobyexample.com/) is a collection of code samples covering a wide variety of features in Go, and includes line-by-line explanations of the code. For those looking for a quick crash course on Go syntax, the Learn X In Y Minutes [Go cheatsheet](https://learnxinyminutes.com/docs/go/) may also be a good starting point.

If you're unconvinced about Go, [the Go playground](https://play.golang.org/) can be used as a sandbox in which to write, build and execute code without installing Go on your machine.

## Using Go
As it builds on the foundations set by many popular and widely-used languages such as C, C++, Java and Python, much of Go's syntax draws from existing implementations and will be familiar to programmers looking to learn an additional language. However, Go also diverges explicitly from these other languages; listed below are some aspects of Go that may be unfamiliar to learners.

### Arrays and slices
Arrays are not often seen in Go code. This is because the size of a Go array must be declared at its creation, which limits how flexible an array can be.

To create dynamically-sized arrays, Go introduces the concept of a slice. Slices are not arrays; rather, they are data structures that describe a piece of a separately-stored array. Slices are the topic of several Go blog entries; you can read more about [the internal layout of slices](https://blog.golang.org/go-slices-usage-and-internals) and [the mechanics of using slices](https://blog.golang.org/slices). Exercises on how to use slices are available in [A Tour of Go](https://tour.golang.org/moretypes/7).

### `defer`, `panic` and `recover`
In addition to traditional control flow mechanisms such as `if`, `for` and `switch`, Go also provides three other ways to alter the flow of a program: `defer`, `panic`, `recover`. These mechanisms can be conceived in terms of exception handling in Java and C++, but also have more general purpose uses.

- `defer` pushes a function call to a list, and executes all functions on the list in last-in-first-out order after the surrounding function returns. It is frequently used for clean-up actions, such as to close files.
- `panic` stops the ordinary flow of execution, executes any deferred functions, and returns to its caller as a call to `panic`. The `panic` continues up the stack until all functions in the goroutine have returned, after which the program crashes.
- `recover` regains control of a panicking goroutine. When used inside a deferred function, a call to recover captures the value returned by `panic` and resumes normal execution.

More information on these three mechanisms and their uses can be found on the [Go blog](https://blog.golang.org/defer-panic-and-recover).

### Interfaces
Although Go has types and methods and allows pseudo-object-oriented style of programming, type hierarchy does not exist in Go. Instead, Go uses interfaces to specify methods that types should implement, grouping types by functionality rather than using notions of inheritance. Types implement interfaces by implementing the methods in the interface, and do not need to explicitly specify which interfaces are implemented.

In the example below, the `Rectangle` type implements the interface `TwoDimensional` by implementing the methods `area()` and `perim()` that are specified in the interface.

```go
type TwoDimensional interface {
  area() float64
  perim() float64
}

type Rectangle struct {
  width, height float64
}

func (r Rectangle) area() {
  return r.width * r.height
}

func (r Rectangle) perim() {
  return r.width * 2 + r.height * 2
}
```

One benefit of using a system where interface implementations need not be stated in the source code is that methods can be attached to types that you didn't write. In other words, you can extend a type to implement an interface without access to its source code by simply implementing the interface's method in your own code.

Some resources to get started with Go interfaces include [this blog post](https://medium.com/golangspec/interfaces-in-go-part-i-4ae53a97479c) introducing Go interfaces, code examples on
[how interfaces (including the empty interface) are used in practice](https://www.calhoun.io/how-do-interfaces-work-in-go/), this [comparison of Go's OOP style with that of other languages](https://flaviocopes.com/golang-is-go-object-oriented/) and [Go's official FAQ on OOP](https://golang.org/doc/faq#Is_Go_an_object-oriented_language).

### Concurrency
One of Go's special features is its support for concurrency. To this end, Go's standard library comes with two features that allow for easy and maintainable concurrency.

#### Goroutines
A goroutine is a lightweight thread that executes a function concurrently with its caller. A goroutine is launched by a `go` statement:

```go
func run() {
  \\ does something
}

func main() {
  go run()
  executeOtherCommands()
}
```

In the above example, using the `go` keyword launches a goroutine, which executes `run()` concurrently with `executeOtherCommands()`.

Goroutines can also be started for anonymous functions:
```go
go func(msg string) {
  fmt.Println(msg)
}("a message")  // starts a goroutine that prints "a message"
```

A goroutine is not its own thread; instead, goroutines are dynamically multiplexed onto threads as required to keep them running. This makes them lightweight, so having a large number of goroutines is feasible. In practice, goroutines behave similarly to very cheap threads.

#### Channels

Channels are used to fulfill Go's philosophy on concurrent software: "don't communicate by sharing memory; share memory by communicating".

Channels are typed conduits that allows goroutines to communicate with each other by sending and receiving messages. Before using a channel of a specific type, we must declare and `make` it:

```go
var c chan int
c = make(chan int)

//alternatively
c := make(chan int)
```

Channels can transmit data of any type; thus, creating a channel that transmits channels (i.e. a `chan chan`) is theoretically possible and may even be [useful](http://tleyden.github.io/blog/2013/11/23/understanding-chan-chans-in-go/).

Goroutines send and receive messages through a channel using the `<-` operator.

```go
ch <- v    // Send v to channel ch.
v := <-ch  // Receive from ch, and
           // assign value to v.
```
One useful way to think about sending and receiving data with the `<-` operator is that the data moves in the direction of the arrow.

If you are interested in delving deeper into using Go's concurrency features extensively, Google developers have put out video presentations on Go's [basic](https://www.youtube.com/watch?v=f6kdp27TYZs) and [advanced concurrency patterns](https://www.youtube.com/watch?v=QDDwwePbDtw). This [code walkthrough](https://golang.org/doc/codewalk/sharemem/) provides an annotated example of how Go's memory-sharing principles can be applied in practice, .

### Effective Go
Formatting in Go is enforced by running `gofmt`, which will align your source code with the language-wide standard style of indentation and vertical alignment. Thus, given the following code:

```go
type T struct {
    name string // name of the object
    value int // its value
}
```
Running `$ gofmt` in the same directory as the source file will line up comments and correct the source code's indentation:

```go
type T struct {
    name    string // name of the object
    value   int    // its value
}
```
Variations on `gofmt` may be of use, and can be found in the [documentation](https://golang.org/cmd/gofmt/).

Go also enforces good coding practices, for instance, by refusing to build projects that declare of unused variables or imports. Such enforcement, along with a clear, unified and extensive [treatise on coding conventions in Go](https://golang.org/doc/effective_go.html), have manifested in a reasonably stable Go coding style.

## Useful resources
Go's development team is heavily involved in documenting and growing the Go language and community. If you are keen to learn more about Go, here are some resources to help you get started:

- [The Go FAQ](https://golang.org/doc/faq) - answers common questions about the language's history, usage, design and more
- [Go's documentation](https://golang.org/doc/) - a good starting point, contains links to official information about Go
- [The Go Blog](https://blog.golang.org/) - features news and in-depth articles about Go by the Go team and guests
- [A list of reasons why Go is bad](https://github.com/ksimka/go-is-not-good) - informative counter-arguments against the use of Go; worth considering before you jump into the world of Go
