---
layout: default
title: Results & Analysis
permalink: /project-1-results
parent: Project 1
nav_order: 1
has_children: false
---

# Cache and Memory Performance Profiling

The code written and used for this project can be found [here](https://github.com/vereimyst/ACS-Project-1).

## Process

Firstly, to obtain any of the measurements required I need to know the specs of the system I am working with. This information can be obtained via a variety of methods. Given the fact that I am working with Linux on WSL2, ChatGPT recommends the following (disregarding the duplicate methods):

1. lscpu | grep -i cache
2. sudo dmidecode -t cache
3. sudo apt-get install hwloc && lstopo
4. getconf -a | grep CACHE

```
$ lscpu | grep -i cache
L1d cache:      384 KiB (8 instances)       -> 48 KiB per instance
L1i cache:      256 KiB (8 instances)
L2 cache:       10 MiB (8 instances)        -> 1.25 MiB per instance
L3 cache:       24 MiB (1 instance)
```

This first method gives a generic overview of what cache structures exist in my laptop, an Alienware m15 R6. Notably, the numbers are rounded and have relatively low precision as a result. The second method unfortunately didn't work due to no SMBIOS nor DMI entry point being found. The third method gives a visual representation of the hierarchical structure that exists internally.

< insert image for cpu.png >
![Image](/assets/images/proj-1/cpu.png)

Again, the capacity values have low precision, though the visualization is quite helpful, allowing us to better understand how each block is connected. The last method gives the highest level of precision as well as some needed details about cache line size and associativity.

```
$ getconf -a | grep CACHE
LEVEL1_ICACHE_SIZE                 32768
LEVEL1_ICACHE_ASSOC
LEVEL1_ICACHE_LINESIZE             64
LEVEL1_DCACHE_SIZE                 49152
LEVEL1_DCACHE_ASSOC                12
LEVEL1_DCACHE_LINESIZE             64
LEVEL2_CACHE_SIZE                  1310720
LEVEL2_CACHE_ASSOC                 20
LEVEL2_CACHE_LINESIZE              64
LEVEL3_CACHE_SIZE                  25165824
LEVEL3_CACHE_ASSOC                 12
LEVEL3_CACHE_LINESIZE              64
LEVEL4_CACHE_SIZE                  0
LEVEL4_CACHE_ASSOC
LEVEL4_CACHE_LINESIZE
```

## Results & Analysis

Blah blah blah

## Citations

ChatGPT used liberally for WSL2 and perf setup.