---
layout: post
title:  "Testing in Go"
date:   2020-11-09 23:00:00
categories: go
---
Go testing support is awesome. It has a subcommand `go test` that looks for `*_test.go` files in package folder and run all test functions that starts with `Test*` prefix and report success or error.

All test functions should follow this naming convention

```go
func TestXXXX(t *testing.T) {

}
```

`t` param is used to report success/failure messages. Some useful methods are
```go
t.Errorf    // to report an error
t.Logf      // to log something
t.Skip      // skip a test
```
More on `testing` package are [here](https://golang.org/pkg/testing/)

Say, we have a package `hello` that defines a simple `Hello` function as follows that we want to add test
```go
// file hello.go
package hello

func Hello() string {
  return "Hello World"
}
```
We need to create `hello_test.go` file in same directory 
```go
// file hello_test.go
package hello

// import testing package
import "testing"

func TestHello(t *testing.T) {
  want := "Hello World"
  if got := Hello(); got != want {
    // report mismatch
    t.Errorf("Hello() = %q, want %q", got, want)
  }
}
```
Now we have tests for our `hello` package. To test we need go to the folder `hello/`
```sh
$ ls
hello.go  hello_test.go
```
If we do `go test` it will run the test runner and execute all `Test*` functions and report success/error
```sh
$ go test
PASS
ok      main/hello  0.011s
```
To see which tests were run we can use verbose option `go test -v`
```sh
$ go test -v
=== RUN   TestHello
--- PASS: TestHello (0.00s)
PASS
ok      main/hello  0.029s
```
We can also specify a substring of specific test function `go test -v -run="Hello"` and test runner will look for test functions that uses the substring
```sh
$ go test -v -run="Hello"
=== RUN   TestHello
--- PASS: TestHello (0.00s)
PASS
ok      main/hello  0.027s
```

__Table driven Testing__

Now we added a `Greet` function that takes a param and return a greet message
```go
func Greet(name string) string {
  return fmt.Sprintf("Hello %s", name)
}
```
Now we want to test `Greet` function but with different test cases, we can create a slice of struct with input, output definition and check results in a loop. This is called table driven testing.
```go
func TestGreet(t *testing.T) {
  testCases := []struct {
    testDesc string
    input string
    output string
  } {
    {"Empty string", "", "Hello "},
    {"ASCII string", "World", "Hello World"},
    {"Unicode string", "世界", "Hello 世界"},
  }
  for _, testCase := range testCases {
    t.Run(testCase.testDesc, func(t *testing.T) {
      if got := Greet(testCase.input); got != testCase.output {
        t.Errorf("Hello() = %q, want %q", got, testCase.output)
      }
    })
  }
}
```
Here we uses an anonymous `struct` that has `testDest`, `input` and `output` fields, and then we create a slice of struct with our test cases.
`t.Run` takes an anynymous function which is similar to our `TestHello` function.

Go does not provide any built assertion library but there are plenty out there to import. one is [testify/assert](https://pkg.go.dev/github.com/stretchr/testify@v1.6.1/assert)

Once we import it from `github.com/stretchr/testify/assert` we can use differnt assertion functions like these 
```go
assert.Equal(t, expected, actual)
assert.NotEqual(t, expected, actual)
assert.True(t, boolStmt)
assert.False(t, boolStmt)
```

Now we can run tests against this test as `go test -v -run="Greet"`
```sh
$ go test -v -run="Greet"
=== RUN   TestGreet
=== RUN   TestGreet/Empty_string
=== RUN   TestGreet/ASCII_string
=== RUN   TestGreet/Unicode_string
--- PASS: TestGreet (0.00s)
    --- PASS: TestGreet/Empty_string (0.00s)
    --- PASS: TestGreet/ASCII_string (0.00s)
    --- PASS: TestGreet/Unicode_string (0.00s)
PASS
ok      main/hello  0.009s
```

Full examples are given in this [repl](https://repl.it/@ShaikhulIslam/testingingo#main.go) link

<iframe height="400px" width="100%" src="https://repl.it/@ShaikhulIslam/testingingo?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>