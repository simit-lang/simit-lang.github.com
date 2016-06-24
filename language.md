---
layout: post
title: The Simit Programming Language 
---
The Simit Programming Language
==============================
___The these pages are under construction___

* auto-gen TOC:
{:toc}

This guide introduces Simit language features and shows how they can be used in
programs. We assume that you are already familiar with imperative programming
and some linear algebra.


# Basics
Simit is an imperative language with statements, control flow and linear
algebra expressions. In this section, we describe some of the language's basic
constructs.

## Functions
A function takes any number of arguments including zero. In the following
example the function `add` takes two arguments of type `float`.

```
func add(a : float, b : float) -> (c : float)
  c = a + b;
end
```

Types follow variable names separated by a `:`.  Further, function result
variables follow the argument list separated by a `->`. Finally, function
bodies are terminated by the `end` keyword, in the same style as Matlab.

## Variables
Variables are declared in function bodies or in global scope using the `var`
keyword. To declare an integer variable you write:

```
var foo : int;
```

If the variable declaration has an initializer then the type can be inferred
from the initializer value. The following statement define a float variable
that is initialized to `0.0`:

```
var foo : float = 0.0;
var foo = 0.0;
```

The `const` keyword creates a variable that is constant after initialization,
which means you can't write to it. The following statements create constant
variables

```
const foo = 0.0;
const bar = foo;
foo = 0.0;
```

Note that variables are const by default, and that eliding the `var`/`const`
keyword leaves the variable as const.

## Basic Types
Simit is statically typed with type inference. This means that the type of
everything is known at compile time, but that you don't have to explicitly
write it if it can be inferred from the right-hand side of an assignment.

The basic types are `bool`, `int`, `float` and `complex`:

```
mybool    : bool    = false;
myint     : int     = 0;
myfloat   : float   = 0.0;
mycomplex : complex = <0.0, 0.0>;
```

The `float` and `complex` types are a double precision floating point by
default when you use the CPU backend, but this can be changed to single
precision floating point when compiling a Simit program. The GPU backend
currently supports only single precision floating point.

## Comments
Single-line comments start with `%`.

```
h = 0.01;  % h is the time-step size
```

Multi-line comments are surrounded by `%{` and `%}`.

```
%{
h is the
time-step size.
%}
h = 0.01;
```


# Control Flow Statements

Simit supports a variety of control flow constructs, including `if` statements,
`while` loops, `do`-`while` loops, and `for` loops.

## If Statements

In Simit, a simple `if` statement looks something like this:

```
if x < 1
  print "x is less than 1";
end
```

An `if` statement can optionally include an `else` clause as well as an
arbitrary number of `elif` (else-if) clauses:

```
if x < 1
  print "x is less than 1";
elif x > 5
  print "x is greater than 5";
else
  print "x is between 1 and 5";
end
```

Simit also supports the following logical operators and comparison operators,
which can be used to construct more complex conditions:

```
x or y;   % Logical OR
x and y;  % Logical AND
x xor y;  % Logical XOR
not x;    % Logical NOT

a == b;   % Equality
a != b;   % Inequality
a < b;    % Less than
a <= b;   % Less than or equal to
a > b;    % Greater than
a >= b;   % Greater than or equal to
```

For example, if we only cared about the scenario in which the value of `x` is
between 1 and 5, then we could have simply written:

```
if x >= 1 and x <= 5
  print "x is between 1 and 5";
end
```

In fact, we can rewrite the above code even more succinctly by chaining the
comparisons:

```
if 1 <= x <= 5
  print "x is between 1 and 5";
end
```

## While Loops

A `while` loop in Simit looks something like this:

```
while x < 100
  x = 2 * x;
end
```

As with `if` statements, logical operators and comparison operators can be used
to construct more complex conditions. Note that if the condition of a `while`
loop is false when a Simit program first encounters the loop, the loop body
will not be executed at all.

## Do-While Loops

A `do`-`while` loop in Simit looks something like this:

```
do
  x = 2 * x;
end while x < 100
```

In contrast to `while` loops, the body of a `do`-`while` loop is guaranteed to
be executed at least once even if the condition is never true.

## For Loops

Simit `for` loops are more like those found in Matlab and Julia than those
available in C. They can be used to iterate over elements in a Simit set. For
example (pun intended):

```
for p in points
  % Do some computation with element p in points set
end
```

You can also use a `for` loop to iterate over the set of all integers between
two values:

```
for i in 0:10
  print i;
end
```

Note that the lower bound is _inclusive_ while the upper bound is _exclusive_,
so the above example prints all integers between 0 and 9 but omits 10.


# Vectors and Matrices
Vector and matrix variables have types, and in the simplest case these just
define the size of each dimension:

```
var vec : vector[3](float);    % A column vector of three floats
var vec : vector[3](float)';   % A row vector of three floats
var mat : matrix[3,3](float);  % A 3x3 matrix of floats
```

Vectors and matrices can also be blocked:

```
var vec : vector[3](vector[2](float));  % A 3-vector with 2-vector blocks
```

There are two types of vector/matrix dimensions. Above we showed vectors and
matrices with simple integer range dimensions. These are much like one- and
two-dimensional arrays in languages like C. But vector and matrix dimensions
can also be Simit sets and we explain this in the [System Vectors and
Matrices](#system-vectors-and-matrices) section.

## Literals
Vector and matrix literals are defined using simple intuitive syntax:

```
% A row 3-vector containing the gravity constant
gravity : vector[3](float) = [0.0, 0.0, 9.8];

% Same as the previous line, but with inferred type
gravity = [0.0, 0.0, 9.8];

% 2x2 matrix of integers
mat = [1, 2; 3, 4];

% 2x1 matrix whose type is inferred
mat21 = [1.0; 2.0];

% 1x2 matrix but cannot infer type since literal is a vector!
mat12 : matrix[1,2](float) = [1.0, 2.0];
```

Note that vector literals are row-major by default! To make a column vector
literal you must transpose them:

```
gravity = [0.0 0.0 9.8]';  % A column 3-vector containing the gravity constant
```

There is also a more regular matrix literal syntax:

```
% 2x2 matrix of integers
mat = [[1, 2], [3, 4]];
```

## Linear Algebra
Simit supports the basic linear algebra expressions, where vectors are
column-oriented by default. The following statements shows several linear
algebra computations:

```
a = b  + c;   % Vector addition
a = b  - c;   % Vector subtraction
a = b';       % Transpose a column vector to a row vector
a = b' * c;   % Inner product
a = b  * c';  % Outer product

A = B + C;    % Matrix addition
A = B - C;    % Matrix subtraction
A = B * C;    % Matrix multiplication

A = (B+C)*d;  % Compound expression

a = b .* c;   % Vector component-wise multiplication
a = b ./ c;   % Vector component-wise division
A = B .* C;   % Matrix element-wise multiplication
A = B ./ C;   % Matrix element-wise multiplication
```

## Indexing
Vectors and matrices are indexed using parentheses with zero-based indexing:

```
var a : vector[2](float);
a(0)   = 0.0;

var A : vector[2,3](float);
A(0,2) = 0.0;
```

Blocked vectors and matrices are indexed using multiple parentheses:

```
var a : vector[3](vector[2)(float));
a(2)(1) = 0.0;
```

## Slicing
You can also use `:` slicing syntax to retrieve a row or column of a matrix:

```
A : matrix[2,2](float) = [1.0, 2.0; 3.0, 4.0];
a : vector[3](float) = A(:,1);  % a == [2.0, 4.0]
```


# System Data Structures
So far we have discussed basic constructs and types. We'll now move on to the
advanced types that are used to describe the structure of whole physical system
and the _global_ properties of these systems. This is the part where Simit
starts to look and feel very different from other languages such as Matlab and
Julia.

In Simit, topology is expressed as a _graph_ data structure, and global
properties are described using system vectors and matrices. A _system_ vector
or matrix is a vector or matrix whose dimension size(s) are proportional to the
size of the whole system, as opposed to an _element_ vector or matrix whose
sizes are a small integer range.

## Elements, Sets and Graphs
Elements, sets and graphs form Simit's _data model_, which is the way you
represent your physical system.

### Elements
An element is a type that stores one or more data fields, and is much like a
struct in a language like C. For example, a point may store a position vector
`x` and a velocity vector `v`, while an element may store a scalar mass:

```
element Point
  x : vector[3](float);
  v : vector[3](float);
end

element Element
  m : float;
  l : float;
end
```

To read or write from or to a field of an Element `e` or a Point `p` you use
`.`:

```
e.m = 2.0;
print e.m;  % 2.0

p.x = [0.0, 0.0, 1.0];
print p.x;  % [0.0, 0.0, 1.0]
```

### Sets
Unlike a C struct, element instantiations live in sets. So instantiations of
the Point element above would live in a `points` set:

```
extern points : set{Point};
```
The `extern` keyword simply means that the verts set comes from outside the
Simit program. This typically means it has been assembled using the Simit C++
library.

The best ways to work with sets and to access their elements is to [apply a
stencil update function](#applying-stencil-update-functions) or to [assemble a
system vector or matrix](#vector-and-matrix-assembly).

### Edge Sets
Edge sets are sets with additional connectivity information. That is, edge set
definitions specify the list of sets that each edge's endpoints come from. The
following declares a set of spring elements that each connect two points:

```
extern springs : set{Element}(points,points);
```

Thus, there is no explicit graph type in Simit; graphs are formed implicitly
from the combination of sets and edge sets. This i similar to the way they are
often defined in mathematical papers `G=(V,E)`.

Simit's graphs are hypergraphs, which just means that edges can have more (or
less) than two endpoints. In other words, each edge can connect n vertices,
where n is any non-negative integer. Thus, we can easily add additional edge
sets that contain triangle, tetrahedral or hexahedral elements:

```
extern triangles  : set{Element}(points,points,points);
extern tetrahedra : set{Element}(points,points,points,points);
extern hexahedra  : set{Element}(points*8);
```
The hexahedra uses a syntactic shortcut that frees us from having to repeat
`points` eight times, but the meaning is the same as if we did.

Edge sets can also be _heterogeneous_, which means that they can connect two or
more _different_ sets. The `links` edge set connects a set of triangles to a
set of tetrahedra:

```
extern links :  set{Link}(triangles, tetrahedra);
```

##Applying Stencil Update Functions
A stencil update function is a function that takes as arguments an element and,
if the element is an edge, it's endpoints. It may read from the element and its
endpoint. If it writes to the element it is called a _pull_ (or gather) stencil
and if it writes to the endpoints then it is called a _push_ (or scatter)
stencil. Stencil update functions are applied to every element of a set
concurrently using an _apply_ expression.

The following update function that takes a point and moves it one unit in the x
direction:

```
func move(p : Point)
  p.x(0) += 1.0;  % p.x += [1.0, 0.0, 0.0];
end
```

It can be applied to the points set using an apply:

```
apply move to points;
```

Since this function is applied to a Point (which is not an edge and has no
endpoints) it can only access the point and therefore only has a local effect.

The following stencil update function that takes a spring, computes its length
and stores that length in the `l` field:

```
func length(s : Element, p : (Point,Point))
  s.l = abs(p(0) - p(1));
end
```
The `p` argument is a tuple containing the two endpoint points of the spring.
Tuples are lists that accessed by integer indices. An alternative tuple syntax
is `p : (Point*2)`.

To apply the function to the springs we use an apply statement.

```
apply length to springs;
```

##System Vectors and Matrices
In addition to integer ranges, the dimensions of vectors and matrices can be a
_set_. We call such vectors and matrices _system_ vectors and matrices and they
can describe properties of an entire physical system. A system vector can be
thought of as a dictionary, while a system matrix can be thought of as a
two-dimensional dictionary (or a dictionary of dictionaries):

```
% A vector with one float per point in points
var a : vector[points](float);

% A vector with one 3-vector block per point in points
var b : vector[points](vector[3](float));

% A matrix with one 3x3 matrix block non-empty per pair of points in points
var K : matrix[points,points](matrix[3,3](float));
```
Note that system matrices are sparse matrices and do not store values for all
|points|^2 combinations.

In the section on set elements we showed how an element field can be read using
a `.`. Sets have fields to, corresponding to the fields of the set elements.
Whereas an set element like point `p` in the set `points` may have a field such
as `x` that is a 3-vector, the `points` sets `x` field is the system vector
that concatenates the `x` field of all its elements.

That is, the type of the vector `p.x` is:

```
vector[3](float)
```

while the type of `points.v` is:

```
vector[points](vector[3](float))
```

## Vector and Matrix Assembly
The key construct that ties sets to global vectors and matrices is the vector
or matrix assembly. Assemblies map a set to a global vector or matrix. Their
semantics are very intuitive and are designed to let the programmer describe
global vectors and matrices as sums of contributions from elements of a set. An
assembly map expression, like the stencil apply, applies a local stencil
function (called an assembly function) to each element of a set. However, each
of these local assembly functions return a global vector and/or matrix that are
added together by the assembly map. That is, an assembly function is a stencil
function that returns one or more global vectors and matrices.

To assemble a force vector each assembly function call returns a global vector
with the force of one element:

```
func tet_force(tet : Element, p : (Point*4))
    -> f : vector[points](vector[3](float))
  for i in 0:4
    f(v(i)) = compute_tet_force(tet,p,i);
  end
end
```

Note that each `tet_force` invocation only has access to four tetrahedra points
and can therefore only write to four locations in the force vector. This is
very important since it prevents indexing errors and lets the compiler produce
fast code since it knows how matrices relate to the graph. However, the
assumption is that your sparse system only directly interacts locally. That is,
if you want two parts to directly interact through a matrix then you need to
connect them with an edge.

The assembly function is applied to the elements of the tetrahedra set using an
assembly map expression to assemble the force vector for the whole system:

```
f = map tet_force to tetrahedra reduce +;
```

To assemble a stiffness matrix each assembly function call returns a global
matrix with the stiffness of one element:

```
func tet_stiffness(tet : Element, p : (Point*4))
    -> K : matrix[points,points](matrix[3,3](float))
  for i in 0:4
    for j in 0:4
     K(v(i),v(j)) = compute_tet_stiffness(tet,p,i,j);
    end
  end
end
```

The assembly function is applied to the elements of the tetrahedra set using an
assembly map expression to assemble the stiffness matrix for the whole system:

```
K = map tet_stiffness to tetrahedra reduce +;
```
