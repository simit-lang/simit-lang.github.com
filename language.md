---
layout: post
title: Language Manual 
---
Language Manual
===============
Simit is organized around graph data structures that you compute on using
linear algebra on global vectors and matrices. For a physical simulation, this
means that you model your physical system as a graph, assemble global vectors
and matrices from this graph, and then compute using linear algebra on these
global vectors and matrices.

The Simit programming system consists of a programming language and a C++ API.
Its design is similar to the OpenGL API, with graphs instead of triangle meshes
and Simit programs instead of shaders.

[The Simit Programming Language](programming-language) lets you write programs
to compute on the graphs using linear algebra. These programs can for example
be used to write a timestepper or the newton's method. It is an imperative
language similar to Matlab, with standard control flow and linear algebra
statements. However, it also has a novel programming language construct that
lets you assemble global vectors and matrices from your graphs.  This construct
is the key to Simit's simplicity, performance and portability.  It makes it
easy to assemble vectors and matrices without indexing bugs. It also lets the
Simit compiler and runtime reason about where matrices come from, what
similarities they share and how they relate to the graphs, which it uses to
optimize for performance.

[The Simit C++ API](api-reference) is the means for interacting with Simit. You
use it to construct the graph data structures that represent your physical
system, to load  and compile programs written in the Simit programming
language, and to run those programs on the graph data structures. The graphs
are built from sets and edge sets and the set elements can store primitive data
types, vectors, matrices and higher-order tensors.
