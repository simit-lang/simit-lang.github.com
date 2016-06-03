---
layout: post
title: Publications 
---
Publications
============

**[Simit: A Language for Physical Simulation](tog16.html)** <br/>
_Fredrik Kjolstad, Shoaib Kamil, Jonathan Ragan-Kelley, David I. W. Levin, Shinjiro Sueda, Desai Chen, Etienne Vouga, Danny M. Kaufman, Gurtej Kanwar, Wojciech Matusik, and Saman Amarasinghe_

With existing programming tools, writing high-performance simulation code is labor intensive and requires sacrificing readability and portability. The alternative is to prototype simulations in a high-level language like Matlab, thereby sacrificing performance. The Matlab programming model naturally describes the behavior of an entire physical system using the language of linear algebra. However, simulations also manipulate individual geometric elements, which are best represented using linked data structures like meshes. Translating between the linked data structures and linear algebra comes at significant cost, both to the programmer and to the machine. High-performance implementations avoid the cost by rephrasing the computation in terms of linked or index data structures, leaving the code complicated and monolithic, often increasing its size by an order of magnitude.

In this article, we present Simit, a new language for physical simulations that lets the programmer view the system both as a linked data structure in the form of a hypergraph and as a set of global vectors, matrices, and tensors depending on what is convenient at any given time. Simit provides a novel assembly construct that makes it conceptually easy and computationally efficient to move between the two abstractions. Using the information provided by the assembly construct, the compiler generates efficient in-place computation on the graph. We demonstrate that Simit is easy to use: a Simit program is typically shorter than a Matlab program; that it is high performance: a Simit program running sequentially on a CPU performs comparably to hand-optimized simulations; and that it is portable: Simit programs can be compiled for GPUs with no change to the program, delivering 4 to 20× speedups over our optimized CPU code.
