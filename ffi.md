---
layout: post
title: Simit FFI 
---
The Simit Foreign Function Interface
====================================
___The these pages are under construction___

The Simit Foreign Function Interface (ffi) lets Simit code call functions written in C.  The `extern` keyword is used to call C functions. Extern functions can accept or return any number of Simit scalar, vector and matrix arguments. Passing elements or sets to extern functions is not supported. The argument list of the C function must contain all the Simit arguments followed by the result arguments, which are passed in using pointers to pointers. A vector and matrix argument passed into an extern function must be matched by one or more arguments describing the vector or matrix type (e.g. it's size) followed by one argument for its value.

The following declares a function `foo` written in C and linked to the program:

```
extern func foo(a : vector[3](int));

export func main()
  vec : vector[3](int) = [1, 2, 3];
  foo(vec);
end
```

The C function `foo` can be defined as follows:

```c
int foo(int n, int* a) {
  if (N > 0) {
    // Print vector values
    printf("[%d", a[0]);
    for (int i=1; i<N; i++) {
      printf(", %d", a[i]);
    }
    printf("]\n");
  }
  return 0;  // Return success error code
}
```
The argument list contains the vector's size followed by a pointer to it's values. The functions returns an error code (0 for success, any other value for failure). This error code is not a part of the Simit program, but a failure will cause an exception to be thrown from the `simit::Function::run` method.

**TODO: System dimensions, matrices, ...**
