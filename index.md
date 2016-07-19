---
layout: post
title: Simit 
---
Simit is a new programming language that makes it easy to compute on sparse
systems using linear algebra. With Simit you build a graph that describes your
sparse system (e.g. a spring system, a mesh or the world wide web). You then
compute on the system in two ways: locally or globally. Local computations
apply update functions to each vertex or edge of the graph that update local
state based on the vertex or the edge and its endpoints. This part of the
language is similar to how GraphLab and its descendants work.

Global computations also start locally with functions applied to each vertex or
edge. However, instead of updating local state the local functions now assemble
a piece of a global vector or matrix. After this assembly, you can compute
using normal linear algebra on the global vectors and matries, and resulting
vectors can be stored back to graph fields. The Simit compiler takes care of
exploiting sparsity and other optimizations, leading to code that is often
simpler than Matlab yet comparable in performance to hand-optimized C++.  Simit
programs compile to both CPUs and GPUs with no changes.

Simit graphs are more flexible than what you find in other graph frameworks. A
Simit graph consists of any number of sets. Some of these are vertex sets while
others are edge sets. Both vertex and edge sets can have data fields, but the
edges of an edge set also connects vertices/edges from other sets. Finally,
edges can connect any number of vertices/edges, which means that graphs in
Simit are hypergraphs.

To learn more about the Simit programming model you can check out our [TOG 2016
paper](tog16), which we will present at SIGGRAPH 2016 in the [Rendering &
Simulation with
GPUs](http://s2016.siggraph.org/technical-papers/sessions/rendering-simulation-gpus)
section on Wednesday 27 July at 3:45--5:55 pm in Ballroom C.

Simit is open source, under a [commercially permissive MIT
license](https://github.com/simit-lang/simit/blob/master/LICENSE). We encourage
you to use it in open source or commercial projects!

If you want announcements and release emails then sign up to the
`simit-announce` email list:
<form action="https://lists.csail.mit.edu/mailman/subscribe/simit-announce" method="POST">
E-mail: <input name="email" /><input type="submit" value="Sign me up" />
</form>
<br/>

