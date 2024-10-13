---
layout: default
title: Project 2
permalink: /project-2
nav_order: 3
# parent: Myst's Projects for ACS
has_children: false
---

# Project #2: Dense/Sparse Matrix-Matrix Multiplication
### Due date: Oct. 12

## Introduction

The objective of this design Project is to implement a C/C++ module that carries out high-speed dense/sparse matrix-matrix multiplication by explicitly utilizing
1. multiple threads,
2. x86 SIMD instructions, and/or
3. techniques to minimize cache miss rate via restructuring data access patterns or data compression (as discussed in class). Matrix-matrix multiplication is one fo the most important data processing kernels in numerous real-life applications, e.g., machine learning, computer vision, signal processing, and scientific computing. This project aims to help you gain hands-on experience of multi-thread programming, SIMD programming, and cache access optimization. It will help you develop a deeper understanding of the importance of exploiting task/data-level parallelism and minimizing cache miss rate.

## Requirements

Your implementation should be able to support configurable matrix size that can be much larger than the on-chip cache capacity. Moreover, your implementation should allow users to individually turn on/off the three optimization techniques (i.e., multi-threading, SIMD, and cache miss minimization) and configure the thread number so that users could easily observe the effect of any combination of these three optimization techniques. Other than the source code, your Github site should contain
1. README that clearly explains the structure/installation/usage of your code.
2. Experimental results that show the performance of your code under different matrix size (at least including 1000x1000 adn 10000x10000) and different matrix sparsity (at elast including 1% and 0.1%).
3. Present and compare the performance of (i) natrive implementation of matrix-matrix multiplications without any optimization, (ii) using multi-threading only, (iii) using SMID only, (iv) using cache miss optimization only, (v) using all the three techniques together.
4. Present the performance of (1) dense-dense matrix multiplication, (2) dense-sparse matrix multiplication, and (3) sparse-sparse matrix multiplication.
5. Thorough analysis and conclusion (include discussions under what matrix sparsity you would like to enable matrix compression).

## Additional Information

C does not have built-in support for multithreaded application, hence must rely on underlying operating systems. Linux provides the pthread library to support multithreaded programming. You can refer to [this very nice tutorial on pthread](https://computing.llnl.gov/tutorials/pthreads/). Microsoft also provides [support for multi-thread programming](https://docs.microsoft.com/en-us/windows/win32/procthread/multiple-threads). Since C++11, C++ has built-in support of multithreading programming, feel free to utilize it. You are highly encouraged to program on Linux since Linux-based programming experience will help you most on the job market.

The easiest way to use SIMD instructions is to call the intrinsic functions in your C/C++ code. The complete reference of the instrinsic functioncs can be found [here](https://software.intel.com/sites/landingpage/IntrinsicsGuide/), and you can find many online materials about their usage.

Moreover matrix-matrix multiplication has been well-studied in the industry, and one well-known library is the Intel Math Kernel Library (MKL), which can be a good reference for you.