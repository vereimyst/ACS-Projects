---
layout: default
title: Project 1
permalink: /project-1
nav_order: 1
---

# Project #1: Cache and Memory Performance Profiling
### Due date: Sept. 25

The objective of this project is to gain deeper understanding of cache and memory hierarchy in modern computers. You should design a set of experiments that will quantitatively reveal the following:
1. the read/write latency of cache and main memory when the queue length is zero (i.e., zero queuing delay)
2. the maximum bandwidth of the main memory under different data access granularity (i.e., 64B, 256B, 1024B) and different read vs. write intensity ratio (i.e., read-only, write-only, 70:30 ratio, 50:50 ratio)
3.  the trade-off between read/write latency and throughput of the main memory to demonstrate what the queuing theory predicts
4.  the impact of cache miss ratio on the software speed performance (the software is supposed to execute relatively light computations such as multiplication)
5.  the impact of TLB table miss ratio on the software speed performance (again, the software is supposed to execute relatively light computations such as multiplication)

The Intel Memory Latency Checker is a useful tool: Google or ask ChatGPT about “Intel Memory Latency Checker”. The Linux “perf” command can gather lots of CPU runtime information such as cache miss ratio and TLB miss ratio, and you can Google or ask ChatGPT to learn more.

Create a Github site to host all your projects through the semester. Post your code/script and detailed results and analysis on Github, and make sure your Github page is clear and self-explanatory.

(The description can also be found in the PDF file attached in the root folder.)
