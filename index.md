---
layout: post
title: Simit 
---
Simit is a new programming language that makes it easy to compute on sparse systems using linear algebra. With Simit you build a graph that describes your sparse system (e.g. a spring system, a mesh or the world wide web). You can then compute on the system in two ways: locally or globally. Local computations apply update functions to each vertex or edge of the graph that update local state based on the vertex or the edge and its endpoints. This is similar to how GraphLab and its descendants work.

Global computations also start locally with functions applied to each vertex or edge. However, instead of updating local state the local functions now assemble a piece of a global vector or matrix. After this assembly, you can compute using normal linear algebra on the global vectors and matries, and resulting vectors can be stored back to graph fields. The Simit compiler takes care of exploiting sparsity and other optimizations, leading to code that is often simpler than Matlab yet comparable in performance to hand-optimized C++.

We plan to release Simit this summer (2016) under a permissive MIT license. If you wish to get release emails and announcements then sign up to the simit-announce email list:
<form action="https://lists.csail.mit.edu/mailman/subscribe/simit-announce" method="POST">
E-mail: <input name="email" /><input type="submit" value="Sign me up" />
</form>
<br/>

In the meantime you can check out our [TOG 2016 paper](tog16), which will be presented at SIGGRAPH 2016 in the [Rendering & Simulation with GPUs](http://s2016.siggraph.org/technical-papers/sessions/rendering-simulation-gpus) section on Wednesday 27 July at 3:45--5:55 pm in Ballroom C.
