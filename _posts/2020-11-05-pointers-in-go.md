---
layout: post
title:  "Pointers in Go"
date:   2020-11-05 23:00:00
categories: pointer, go
---

Go is a call-by-value language, meaning it will use a copy of each function arguments,  if we want to update a variable we need to pass its address using a pointer.
Go pointer syntax are similar as C.

__Pointer basics__
Get address of a variable using `&` operator
Derefence using pointer `*` operator
```go
// int variable
year := 2020
// we can get address using & op
year_add := &year
fmt.Println("Val of year: ", year)
fmt.Println("Address of year: ", &year)
fmt.Println("Val of year using pointer: ", *(&year))
```

__Function call by value__
```go
func call_by_val(year int) {
  year = year + 1
}
call_by_val(year)
// year won't be updated as it uses a copy in call_by_val function
fmt.Println("Call by val res: ", year)
```

__Function cal by reference__
```go
func call_by_ref(year *int) {
  *year = *year + 1
}
call_by_ref(&year)
// here it will update the variable as we pass the address of the variable
fmt.Println("Call by ref res: ", year)
```

__Method with Pointer Receiver__
When using pointer receiver in methods, Go compiler does implicit conversion to match the type of receiver argument with receiver parameter.
To explain different use cases I will use a `Point` type and some methods as follows
```go
type Point struct {
  X, Y int
}
func (p *Point) Scale(factor int) {
  p.X = p.X * factor
  p.Y = p.Y * factor
}
// pointer receiver param
func (p *Point) Print() {
  fmt.Println("X: ", p.X, ", Y: ", p.Y)
}

// receiver param of type Point, any change won't be reflected on receiver argument
func (p Point) ScaleVal(factor int) {
  p.X = p.X * factor
  p.Y = p.Y * factor
}

// value receiver param
func (p Point) PrintP() {
  fmt.Println("X: ", p.X, ", Y: ", p.Y)
}
```
* Use case 1: both uses same pointer type
No implicit conversion
```go
p := Point{X:2, Y:3}
p_ptr := &p
Scale(p_ptr, 2)
// this will print the point by 2
Print(p_ptr)
```

* Use case 2: receiver param is of type pointer 
```go
// receiver arg is of type Point, compiler implicitly get &p to match type with receiver param
p.Scale(2)
p.Print()
```

* User case 3: receiver argument is of type pointer
```go
// receiver param is of type Point but receiver arg is a pointer, compiler dereference the receiver but any mutation won't be reflected on receiver arg
p_ptr.ScaleVal(2)
p_ptr.PrintP()
```

<iframe height="400px" width="100%" src="https://repl.it/@ShaikhulIslam/pointersingo?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>