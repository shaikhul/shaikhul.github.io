---
layout: post
title:  "Go Basics: Arrays, Slices and Maps"
date:   2020-11-11 14:00:00
categories: go, array, slice, map
---
### Go Arrays, Slices and Maps

#### Arrays
Go arrays are fixed-length consecutive elements of a specific type.

```go
// array declaration
var nums [3]int
fmt.Println(nums)

// two-d array
var twoDArray [3][3]int = [3][3]int{
{1, 2, 3},
{4, 5, 6},
{7, 8, 9},
}
fmt.Println(twoDArray)

// arra declare + init
var fruits [3]string = [3]string{"Apple", "Orrange", "Mango"}
fmt.Println(fruits)

// inferred
vegetables := [...]string{"Broccoli", "Cabbage", "Celery", "Kale", "Lettuce"}
fmt.Println(vegetables)

// zero values of given type
var otherNums [2]int

// array size is part of the type, array of two different size are not comparable
fmt.Printf("%T, %T, %T, %T\n", nums, otherNums, fruits, vegetables)
```

#### Slices
Go Slices are dynamic sized elements of a given type. Its declaration almost similar to array without the size explicitly given. Slice are closely related to Array. Slices represent a sub sequence of an underlying array. Multiple slice can point to a particular array.
```go
var aSlice []int = []int{1, 2, 3, 4, 5}
fmt.Printf("vals:%v, type: %T, len: %d, cap: %d\n", aSlice, aSlice, len(aSlice), cap(aSlice))

veg_slice_first_two := vegetables[:2] // len: 2, cap: 5
veg_slice_last_three := vegetables[2:] // len: 3, cap: 3
fmt.Println(veg_slice_first_two, veg_slice_last_three)
fmt.Printf("len - %d, cap - %d\n", len(veg_slice_first_two), cap(veg_slice_first_two))
fmt.Printf("len - %d, cap - %d\n", len(veg_slice_last_three), cap(veg_slice_last_three))

// 2d slice
var twoDSlice [][]int = [][]int{
{1, 2, 3},
{4, 5},
}
fmt.Println(twoDSlice)

// zero value of slice is nil
var unInitSlice []int
fmt.Println(unInitSlice == nil)

// empty slice has len = 0 but not nil
var emptySlice []int = []int{}
fmt.Println(len(emptySlice) == 0)
fmt.Println(emptySlice != nil)
```

#### Maps
Go Maps are key/value data structure, where all keys must be of same type and all values must of same type.
```go
// map literal
var dict map[string]string = map[string]string {
"name": "go",
"version": "1.14",
}
fmt.Println(dict)

// using make builtin
user := make(map[string]string)
user["name"] = "foo"
user["email"] = "foo@bar.com"
fmt.Println(user)

// map element access by key returns a tuple (val, bool), where bool represents key exists or not
email, ok := user["email"]
if !ok {
fmt.Println("key does not exist")
}
fmt.Println(email)

// zero values of map is nil
var zeroValMap map[int]int
fmt.Println(zeroValMap == nil)

// empty map
var emptyMap map[int]int = map[int]int{}
fmt.Println(emptyMap != nil)
```

All examples are given [here](https://repl.it/@ShaikhulIslam/goarraysslicesmaps)

<iframe height="400px" width="100%" src="https://repl.it/@ShaikhulIslam/goarraysslicesmaps?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>