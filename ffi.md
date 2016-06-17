---
layout: post
title: Simit FFI 
---
The Simit Foreign Function Interface (ffi) 
==========================================
___The these pages are under construction___

The foreign function interface (ffi) lets Simit code call functions written in
C.  The `extern` keyword is used to call C functions.  The following
declaration declares a function `foo` that must be written in C and linked to
the program:

```
extern func foo(a : vector[3](int));

export func main()
  vec : vector[3](int) = [1, 2, 3];
  foo(vec);
end
```

The C function `foo` can be defined as follows:

```c
void foo(int* a) {
  printf("[%d, %d, %d]", a[0], a[1], a[2]);
}
```

Note that there are no arguments describing the size of the `a` array.  This is
because the foo declaration was of a vector with a static size.  To get a size
argument declare the `extern` function using a `*` dimension:

```
extern func foo(a : vector[*](int));
```

```c
void foo(int N, int* a) {
  if (N == 0) return;
  printf("[%d", a[0]);
  for (int i=1; i<N; i++) {
    printf(", %d", a[i]);
  }
  printf("]\n");
}
```

**TODO: System dimensions, matrices, ...**
