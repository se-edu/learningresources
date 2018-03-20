# Introduction to Go

Author(s): [Cara Leong](https://github.com/craaaa)

Go (also known as `golang`) is a compiled, statically-typed, garbage-collected language that has special memory safety and concurrent programming features. Born out of frustration with the available languages (e.g. C, C++, Java) and environments for systems programming, Go was [conceptualized by programmers at Google](https://talks.golang.org/2012/splash.article) who sought to create a single language that was efficient to write, build and execute. Go also supports newer developments in computing such as multicore processors and network systems.

Go is an [open source project](https://github.com/golang/go). Its source code may be useful reading for those interested in learning good practices, or simply to find out more about how the language was implemented.

## Getting started

Go provides its own [installation guide](https://golang.org/doc/install) and an interactive [ tour of Go](https://tour.golang.org/). These are useful and highly comprehensive resources for programmers looking to learn the syntax and style of Go. For those who prefer to read existing code examples, [Go by Example](https://gobyexample.com/) is a collection of code samples covering a wide variety of features in Go, and includes line-by-line explanations of the code. For those looking for a quick crash course on Go syntax, the Learn X In Y Minutes [Go cheatsheet](https://learnxinyminutes.com/docs/go/) may also be a good starting point.

If you're unconvinced about Go, [the Go playground](https://play.golang.org/) can be used as a sandbox in which to write, build and execute code without installing Go on your machine.

## Using Go
As it builds on the foundations set by many popular and widely-used languages such as C, C++, Java and Python, much of Go's syntax draws from existing implementations and will be familiar to programmers looking to learn an additional language. However, Go also diverges explicitly from these other languages; listed below are some aspects of Go that may be unfamiliar to learners.

### Declaring Variables
One immediately obvious difference between Go and other languages is that Go declares its variables in the format `var variableName (variableType)`, with the variable's type declared to the right of the variable name, as follows:

```go
var name string
name = "John Smith"
```
This differs from many other languages, which put the variable type to the left of the variable name.

In addition, the length of a declaration statement in Go can be variable. The `variableType` of a new variable need not be declared if an initializer is used. For instance, in the following example, the `string` type is optional:

```go
var name string = "John Smith"

// alternatively
var name = "John Smith"
```

When initializers are not used, simply declaring a variable allocates storage for the variable and initializes its value to that type's zero value. The zero value is `0` for numeric types, `false` for booleans, `""` for strings, `nil` for pointers, functions, interfaces, slices, channels, and maps.

As an extension of type inference, the `:=` short assignment statement can be used inside a function to as a `var` declaration with implicit type:
```go
name := "John Smith"
```

Thus, we see that there are several potential ways to declare a variable in Go. Choosing which style of variable declaration to use depends on how verbose a programmer wants to be in declaring the variable.

Go's syntax for more complex types such as pointers, arrays and structs is also somewhat idiosyncratic, and can be explored in this [Go blog article](https://blog.golang.org/gos-declaration-syntax), or with the help of [this tutorial on pointers](http://www.golang-book.com/books/intro/8) and [this tutorial on structs](https://www.golang-book.com/books/intro/9).

### Arrays and Slices
Arrays are not often seen in Go code. This is because the size of a Go array must be declared at its creation, which limits how flexible an array can be. For instance, in the 

To create dynamically-sized arrays, Go introduces the concept of a slice. Slices are not arrays; rather, they are data structures that describe a piece of a separately-stored array. Slices are the topic of several Go blog entries; you can read more about [the internal layout of slices](https://blog.golang.org/go-slices-usage-and-internals) and [the mechanics of using slices](https://blog.golang.org/slices). Exercises on how to use slices are available in [A Tour of Go](https://tour.golang.org/moretypes/7).

### `defer`
As opposed to traditional control flow mechanisms such as `if`, `for` and `switch`, which execute functions immediately, Go's `defer` keyword pushes a function call to a list, and only executes all functions on the list after the surrounding function returns. In the following example, `defer` adds two print functions to the stack of deferred functions. After `foo` finishes executing, the deferred functions are executed in last-in-first-out order.

```go
func foo() {
	defer fmt.Println("This gets printed third")
	defer fmt.Println("This gets printed second")
	fmt.Println("This gets printed first")
}
```

`defer` is frequently used for clean-up actions, such as to [close files](https://gobyexample.com/defer). Deferred functions run on panicking goroutines as well, which makes them useful for recovering from `panic`.

### Error Handling
Error handling in Go is performed using multiple returns. On any function that can fail, the function's last return type should always be of the type `error`. For example, the `os.Open` function returns a non-nil error value when it fails to open a file.

```go
func Open(name string) (file *File, err error)
```

An `error` variable represents any value that can describe itself as a string, by implementing the following interface:

```go
type error interface {
	Error() string
}
```

When calling a method that may return an error, we check if the returns `err != null` and handle the resulting error.

```go
func useFile() {
	f, err := os.Open("filename.ext")
	if err != nil {
	    log.Fatal(err)
	    return
	}
	// do something with the open *File f
}
```

To deal with unexpected errors, Go also provides two mechanisms: `panic` and `recover`.

- `panic` stops the ordinary flow of execution, executes any deferred functions, and returns to its caller as a call to `panic`. The `panic` continues up the stack until all functions in the goroutine have returned, after which the program crashes. `panic` is used to fail fast on errors that cannot be handled gracefully.
- `recover` regains control of a panicking goroutine. When used inside a deferred function, a call to recover captures the value returned by `panic` and resumes normal execution.

More information on error handling can be found on the [Go blog](https://blog.golang.org/error-handling-and-go).

### Interfaces
Although Go has types and methods and allows pseudo-object-oriented style of programming, type hierarchy does not exist in Go. Instead, Go uses interfaces to specify methods that types should implement, favouring composition over inheritance. Types implement interfaces by implementing the methods in the interface, and do not need to explicitly specify which interfaces are implemented.

In the example below, the `Rectangle` type implements the interface `TwoDimensional` by implementing the methods `area()` and `perim()` that are specified in the interface. Thus, instances of `Rectangle` can be used as arguments to `price`. 

Meanwhile, although `Circle` implements `perim()`, it does not implement `area()`. Since it does not implement all the methods in the `TwoDimensional` interface, `Circle` does not implement `TwoDimensional`. Thus, instances of `Circle` cannot be used as arguments to `price`.

```go
type TwoDimensional interface {
	area() float64
	perim() float64
}

type Rectangle struct {
	width, height float64
}

type Circle struct {
	radius float64
}

func (r Rectangle) area() {
	return r.width * r.height
}

func (r Rectangle) perim() {
	return r.width*2 + r.height*2
}

func (c Circle) perim() {
	return 2 * math.Pi * c.radius
}

func price(t TwoDimensional) {
	return t.area() * 3.5
}
```

One benefit of using a system where interface implementations need not be stated in the source code is that methods can be attached to types that you didn't write. In other words, you can extend a type to implement an interface without access to its source code by simply implementing the interface's method in your own code.

Some resources to get started with Go interfaces include [this blog post](https://medium.com/golangspec/interfaces-in-go-part-i-4ae53a97479c) introducing Go interfaces and code examples on
[how interfaces (including the empty interface) are used in practice](https://www.calhoun.io/how-do-interfaces-work-in-go/). For a more extensive look at how object-oriented programming is done in Go, you can refer to this [comparison of Go's OOP style with that of other languages](https://flaviocopes.com/golang-is-go-object-oriented/), [Go's official FAQ on OOP](https://golang.org/doc/faq#Is_Go_an_object-oriented_language), or this [tutorial on OOP in Go](https://code.tutsplus.com/tutorials/lets-go-object-oriented-programming-in-golang--cms-26540).

### Concurrency
One of Go's special features is its support for concurrency. To this end, Go's standard library comes with two features that allow for easy and maintainable concurrency.

#### Goroutines
A goroutine is a lightweight thread that executes a function concurrently with its caller. A goroutine is launched by a `go` statement:

```go
func run() {
	// does something
}

func main() {
	go run()
	executeOtherCommands()
}
```

In the above example, using the `go` keyword launches a goroutine, which executes `run()` concurrently with `executeOtherCommands()`.

Goroutines can also be started for anonymous functions:

```go
func foo() {
	go func(msg string) {
		fmt.Println(msg)
	}("a message") // starts a goroutine that prints "a message"
}
```

A goroutine is not its own thread; instead, goroutines are dynamically multiplexed onto threads as required to keep them running. In addition, goroutines start with [very small stacks](https://stackoverflow.com/questions/41906357/why-are-goroutines-much-cheaper-than-threads-in-other-languages) which makes them lightweight, so having a large number of goroutines is feasible. In practice, goroutines behave similarly to very cheap threads.

#### Channels

Channels are used to fulfill Go's philosophy on concurrent software: "don't communicate by sharing memory; share memory by communicating". In other words, Go relies on message passing between concurrently running goroutines to share information.

Specifically, Go relies on channels to implement message passing. Channels are typed conduits that allows goroutines to communicate with each other by sending and receiving messages. Before using a channel of a specific type, we must declare and `make` it:

```go
var c chan int
c = make(chan int)

//alternatively
c := make(chan int)
```

Channels can transmit data of any type; thus, creating a channel that transmits channels (i.e. a `chan chan`) is theoretically possible and may even be [useful](http://tleyden.github.io/blog/2013/11/23/understanding-chan-chans-in-go/).

Goroutines send and receive messages through a channel using the `<-` operator.

```go
ch <- v   // Send v to channel ch.
v := <-ch // Receive from ch, and
	  // assign value to v.
```
One useful way to think about sending and receiving data with the `<-` operator is that the data moves in the direction of the arrow.

Channels can be used to synchronize execution across goroutines, since receivers block until they receive data, while senders block [until the receiver or buffer receives data](https://golang.org/doc/effective_go.html#channels). In the code example below, the `main` goroutine waits until it receives a message from the `worker` goroutine that it is done before terminating.

```go
func worker(done chan bool) {
	fmt.Print("Working...")
	time.Sleep(time.Second)
	fmt.Println("Done working")

	done <- true
}

func main() {
	done := make(chan bool)
	go worker(done)

	<-done
	fmt.Println("Returned from work")
}
```

If you are interested in delving deeper into using Go's concurrency features extensively, Google developers have put out video presentations on Go's [basic](https://www.youtube.com/watch?v=f6kdp27TYZs) and [advanced concurrency patterns](https://www.youtube.com/watch?v=QDDwwePbDtw). This [code walkthrough](https://golang.org/doc/codewalk/sharemem/) provides an annotated example of how Go's memory-sharing principles can be applied in practice.

### Canonical Coding Style
Formatting in Go is enforced by running `go fmt`, which will align your source code with the language-wide standard style of indentation and vertical alignment. Thus, given the following code:

```go
type T struct {
    name string // name of the object
    value int // its value
}
```
Running `$ go fmt` in the same directory as the source file will line up comments and correct the source code's indentation:

```go
type T struct {
	name  string // name of the object
	value int    // its value
}
```
Variations on `go fmt` may be of use, and can be found in the [documentation](https://golang.org/cmd/gofmt/).

Go also enforces good coding practices, for instance, by refusing to build projects that declare of unused variables or imports. Such enforcement, along with a clear, unified and extensive [treatise on coding conventions in Go](https://golang.org/doc/effective_go.html), have manifested in a reasonably stable Go coding style.

## Useful Resources
Go's development team is heavily involved in documenting and growing the Go language and community. If you are keen to learn more about Go, here are some resources to help you get started:

- [The Go FAQ](https://golang.org/doc/faq) - answers common questions about the language's history, usage, design and more
- [Go's documentation](https://golang.org/doc/) - a good starting point, contains links to official information about Go
- [The Go Blog](https://blog.golang.org/) - features news and in-depth articles about Go by the Go team and guests
- [A list of reasons why Go is bad](https://github.com/ksimka/go-is-not-good) - informative counter-arguments against the use of Go; worth considering before you jump into the world of Go
