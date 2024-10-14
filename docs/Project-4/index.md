---
layout: default
title: Project 4
permalink: /project-4-desc
nav_order: 5
has_children: true
---

# Project #4: In-Memory Key-Value Store with Concurrency Support
### Due date: Nov. 15

## Introduction
The goal of this project is to design and implement an in-memory key-value store that supports concurrent operations, ensuring data consistency and efficiency. The store will handle multiple read and write operations simultaneously, employing appropriate synchronization mechanisms to avoid race conditions and ensure correct behavior in a multithreaded environment. In-memory key-value stores are widely used for high-performance applications where low-latency data retrieval is crucial. They are commonly used in caching systems, real-time analytics, and distributed systems. In this project, you will build a simplified version of an in-memory key-value store from scratch, focusing on efficient data storage, retrieval, and concurrency control.

## Requirement
Your implementation should support the following operations:
1. Put(key, value): Inserts or updates a key-value pair
2. Get(key): Retrieves the value associated with a given key
3. Delete(key): Removes a key-value pair from the store
4. Support for multiple concurrent read and write operations
5. Optional (as a bonus): Support lossless in-memory data compression at small speed performance loss

Feel free to choose your preferred in-memory indexing data structure (hash, tree, etc.). You should also write a testbench to test your in-memory key-value store under
1. different operational concurrency (1 user, 2 users, 4 users, ...)
2. different number of internal working threads (4, 8, 16, ...)
3. different read vs. write ratio
4. different value size (8B, 64B, 256B) - you may keey the key size as 8B

Again, post all your code, README, report, and analysis on your GitHub site.