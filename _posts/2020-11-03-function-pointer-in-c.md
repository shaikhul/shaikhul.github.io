---
layout: post
title:  "Function Pointer in C"
date:   2020-11-03 20:46:00
categories: personal
---

Pointers in C are powerful and confusing at the same time. As we can point a pointer to structs, strings or arrays, we can point to function.

__Signature:__ `void (*POINTER_NAME) (args)`

Say for example we have a function add that adds two number `int add(int a, int b)`

```c
int add(int a, int b) {
    return a + b;
}

int mul(int a, int b) {
    return a * b;
}
```

The function pointer for this would be
```c
int (*fun_ptr) (int, int)
```

We can assign this pointer to any function that accepts two `int` params and return an `int`.

To point to `add` function we can just assign it to the `fun_ptr` pointer as follows.

Now we can use the function pointer as follows
```c
// assign it to add function
fun_ptr = add;
printf("2 + 3 = %d\n", fun_ptr(2, 3));

// assign it to mul function
fun_ptr = mul;
printf("2 * 3 = %d\n", fun_ptr(2, 3));
```

Now if we want to use function pointer as a function param we can use it directly or use a `typedef` as follows

```c
// it takes a function pointer as param
int do_op(int a, int b, int (*ptr) (int, int)) {
  return ptr(a, b);
}
printf("2 + 3 = %d \n", do_op(2, 3, add));
printf("2 + 3 = %d \n", do_op(2, 3, mul));
```

As writing the function signature as a function param is hard, we can use the `typedef` to give it a new name
```c
// using typedef
typedef int (*op_ptr_type) (int, int);
int do_op2(int a, int b, op_ptr_type ptr) {
  return ptr(a, b);
}
printf("2 + 3 = %d\n", do_op2(2, 3, add));
  printf("2 * 3 = %d\n", do_op2(2, 3, mul));
```
Here is the complete example in [repl.it](https://repl.it/@ShaikhulIslam/functionpointers)

<iframe height="400px" width="100%" src="https://repl.it/@ShaikhulIslam/functionpointers?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>

Now, lets see how [qsort](https://en.cppreference.com/w/c/algorithm/qsort) takes advantage of function pointer.


The method signature of `qsort` is
```c
void qsort(void *ptr, size_t count, size_t size, int (*comp)(const void *, const void *));
```

Here we can see the fourth param is a callback which is used to do custom comparison. If we want to sort some numbers in ascending order we need to define the `comp` function first using the same signature
```c
int comp_int_asc(const void* a, const void* b) {
    // dereference to int
    int val1 = *(const int*)a;
    int val2 = *(const int*)b;

    if (val1 < val2) {
        return -1;
    } else if (val1 > val2) {
        return 1;
    }
    return 0;
}
```
we can use it in the `qsort` function as follows
```c
int nums[] = {5, 1, 3, 9, 10, 8, -5, 0};
int int_size = sizeof(int);
int arr_len = sizeof(nums) / int_size;
// qsort is defined in stdlib
qsort(nums, arr_len, int_size, comp_int_asc);
```
[Here]((https://repl.it/@ShaikhulIslam/qsortc#main.c)) is the full qsort example

<iframe height="400px" width="100%" src="https://repl.it/@ShaikhulIslam/qsortc?lite=true" scrolling="no" frameborder="no" allowtransparency="true" allowfullscreen="true" sandbox="allow-forms allow-pointer-lock allow-popups allow-same-origin allow-scripts allow-modals"></iframe>
