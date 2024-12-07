---
layout: default
title: Results & Analysis
permalink: /project-5-results
parent: Project 5
nav_order: 1
has_children: false
---

# Final Project

The code written and used for this project can be found [here](https://github.com/vereimyst/ACS-Project-5). I chose this topic since it is in line with my intern experience at AMD earlier this year.

## Process

All processors have a fundamental component called the Arithmetic Logic Unit (ALU), which performs basic arithmetic operations (e.g. addition, subtraction) and logical operations. Our computers are dependent on this block for everything from instruction decode and execution to memory addressing. Even Floating-Point Units (FPUs), hardware components that are becoming increasingly important for larger scale/faster AI development [1][2], are, at the core, dependent on adders. This makes optimizing the efficiency (speed, size, and power) of adders crucial. One clear and commonly used method of doing this is approximation. However, even then many approximation methods exist:
- lookup tables (LUTs),
- bit truncation,
- reduced carry propagation,
- dynamic precision scaling,
- significance-driven,
- hybrid (combining exact and approximate logic within same circuit),
- stochastic,
- voltage overscaling (VOS),
- neural (network)-inspired,
- simplified logic gates,
- custom arithimetic circuits, 
- probabilistic computing,
- error tolerance adders (ETA), etc.

Due to both the limitations some of these techniques have and the challenges other techniques pose in terms of implementation and testing, I went with the following methods:
1. bit truncation (bit masking)
    - Pros:
        - computationally cheap, complexity scales proportionally to bit width
        - easy to implement (no additional logic, low overhead)
        - widely applicable if lower bits less important (e.g. image processing, machine learning)
    - Cons:
        - less adaptive, doesn't consider dynamic input importance (e.g. dynamic precision scaling or significance-driven)
        - unsuitable for high precision applications (e.g. floating point arithmetic)
2. reduced carry propagation
    - Pros:
        - fine-grained control, balances precision and complexity
        - more scalable (large bit widths) than some methods
    - Cons:
        - more specific to adders, less applicable on a broader scale (of logic operations) than voltage scaling or approximate multipliers 
        - less dynamic than some other techniques (e.g. probabilistic carry handling)
3. stochastic
    - Pros:
        - introduces randomness (mimics real-world noise)
        - allows for error control and tuning approximation level using parameters
    - Cons:
        - higher implementation complexity
        - requires random number generation hardware/software
        - difficult to mitigate randomness overhead at large scales
4. simplified logic gates
    - Pros:
        - direct logic replacement/reduction in circuit complexity without redesigning overall (hardware) structure saves energy and area
        - simpler than techniques involving pre-computed lookup tables or segmented adders
    - Cons:
        - scalable with constraints as higher precision accumulates error
        - less versatile than hybrid approaches that take into account input significance

I also elected to start with a simple full adder to allow for better understanding of the impact of the approximation techniques, with later testing on a ripple carry to better simulate real-world logic. Realistically, most modern processers would use more structurally complex and computationally streamlined adders (e.g. carry lookahead). Lastly, I chose some common bit-widths that would be used in computations: 2, 4, 8, 16, 32, and 64.

Starting with a software simulation of the performance using Python, I wanted to get a better idea of exactly how each approximation technique worked. This allows us to see the behavior of each technique in an ideal setting, with no hardware contingencies as some have more convoluted implementations. (Note: The graphs included on my presentation were using different adder implementations and a different testbench, which I later realized measured each techniques performance incorrectly.)

![software simulation relative error vs bit-width]({{ site.url }}/assets/images/proj-5/sw-sim-error-across-techniques.png)

![software simulation execution time vs bit width]({{ site.url }}/assets/images/proj-5/sw-sim-time-across-techniques.png)

This gives us some interesting general trends for relative error. The highest relative error overall is with bit truncation, which makes sense since we are continuously ignoring the same number of bits. For the smallest bit-width, 2-bit operations, there is more irregular behavior and stochastic ends up having the highest error. Comparatively, the lowest relative error comes from simplified logic gates, though stochastic comes close as bit-widths increased since I made the error rate scale on bit-width. Most importantly, it appears as though the error eventually plateaus out beyond 16 bits, which we know is not true in hardware. For execution time, we see that it scales linearly with bit width, with stochastic consistently taking longer than all other techniques and simplified logic gates being fastest. This also lines up with our expectations as stochastic has the added component of the randomness overhead as mentioned previously. 

Then, I tried simulating the techniques using Python's PyMTL3, an open-source Python-based hardware generation, simulation, and verification framework. I restricted the range of values that addition was performed on for the various bit-widths since calculations took far too long (10+ minutes to run a single adder). The values I selected result in some variation from run to run, but general trends stay consistent and execution time is fast.

![hardware simulation absolute error vs bit-width]({{ site.url }}/assets/images/proj-5/hw-sim-abs-err-across-bit-widths.png)

![hardware simulation absolute error vs bit-width]({{ site.url }}/assets/images/proj-5/hw-sim-abs-err-across-bit-widths-scaled.png)

![hardware simulation relative error vs bit-width]({{ site.url }}/assets/images/proj-5/hw-sim-rel-err-across-bit-widths.png)

![hardware simulation relative error vs bit-width (scaled)]({{ site.url }}/assets/images/proj-5/hw-sim-rel-err-across-bit-widths-scaled.png)

Each graph is accompanied by a scaled graph as stochastic has a vastly higher error rate than all other techniques. From highest to lowest error rate (overall):
1. stochastic
2. truncated
3. simplified
4. reduced

This can be attributed to the randomness component that is introduced in calculation. Subsequent observations refer to scaled graphs. We see that truncated has generally the highest error, as expected, since we are ignoring a portion of the bit values. Reduced has the highest accuracy, also as expected. Simplified likely has higher inaccuracy due to the calculation using XOR, however, it should be increasing as bit widths increases, which we don't see.

![hardware simulation time vs bit-width]({{ site.url }}/assets/images/proj-5/hw-sim-time-across-bit-widths.png)

A little hard to see, but the overall execution time ranking (highest to lowest) goes:
1. exact
2. reduced
3. truncated
4. stochastic
5. simplified
These results line up perfectly with what we expected. Exact takes the longest, hence approximation techniques are used to improve efficiency. Reduced has the highest precision of the methods tested, which results in having the highest runtime. Truncated has highest error but medium runtime, likely due to fact that I elected to mask half the bit-width for each. Stochastic has an impressively low runtime, likely due to low overhead for randomness since we are working with generally lower bit-widths and I used software generated randomness. I tried re-running stochastic using Linear Feedback Shift Registers (LFSRs) as the randomness generator, wherein we can see that the stochastic execution time varies and begins to climb as bit-width increases (refer to graphs below). Lastly, simplified has the lowest execution time as we are working with simplified gates (XOR instead of OR).

![hardware simulation absolute error vs bit-width w/ alternate stochastic implementation]({{ site.url }}/assets/images/proj-5/hw-sim-abs-err-across-bit-widths-alt.png)

![hardware simulation relative error vs bit-width w/ alternate stochastic implementation]({{ site.url }}/assets/images/proj-5/hw-sim-rel-err-across-bit-widths-alt.png)

![hardware simulation time vs bit-width w/ alternate stochastic implementation]({{ site.url }}/assets/images/proj-5/hw-sim-time-across-bit-widths-alt.png)

I exported my implementations through PyMTL's included conversion to Verilog function, but was unable to successfully run the simulation in Vivado. The RTL did, however, successfully synthsize and below you can see the utilization for the implementations on a XC7A35TCSG324-1 device (as recommended by ChatGPT for my project).

![Vivado utilization]({{ site.url }}/assets/images/proj-5/utilization.png)

## Academic Research & Real-World Application

From even my small scale research, it is clear that there are many benefits to using approximation techniques in calculations. Exact application cases vary depending on the technique and what the calculation results are being used for. For example, if we require higher precision, we should use reduced propagation as opposed to stochastic, whereas if we prioritize faster runtime and care little for accuracy, bit truncation (with more bits masked) or stochastic (closer to real-world conditions) would be good choices. I came across a variety of papers testing unique adder structures when I was trying to get a better idea of this topic's relevance. All noted significant improvements in delay, power, and area with varying error rates, depending on the approximation adder used:
- Ripple Carrying Approximate 1-bit Adders --> 30% reduction in power with 15% Root-Mean-Square (RMS) error [4]
- Ripple Carrying Approximate Full Adders (AFAs) --> 46.31% reduction in power and 28.57% reduction in area [5]
- Ripple Carrying Approximate Carry Speculative Han-Carlson Adder (ACSHA) --> 46% improvement in Area Delay Product (ADP) with negligible relative error [6]

The problem we strive to solve is getting the highest precision with the lowest area, time, and power usage. The last paper in particular indicates recent research regarding new approximation methods, where we seem to have much space for innovation still.

## Citations

ChatGPT used liberally for all parts.

[1] https://medium.com/@sasirekharameshkumar/ai-processors-cpu-gpu-tpu-npu-ba7014f32bc2#:~:text=1.,matrix%20operations%20in%20deep%20learning.

[2] https://semiengineering.com/artificial-intelligence-chips-must-get-the-floating-point-math-right/

[3] https://www.csl.cornell.edu/~cbatten/misc/brg-pymtl-tutorial-princeton2018.pdf

[4] C. I. Allen, D. Langley and J. C. Lyke, "Inexact computing with approximate adder application," NAECON 2014 - IEEE National Aerospace and Electronics Conference, Dayton, OH, USA, 2014, pp. 21-28, doi: 10.1109/NAECON.2014.7045768. keywords: {Logic gates;Adders;Approximation methods;Inverters;Probabilistic logic;Power dissipation;Minimization},

[5] Sunil Dutt, Sukumar Nandi, and Gaurav Trivedi. 2017. Analysis and Design of Adders for Approximate Computing. ACM Trans. Embed. Comput. Syst. 17, 2, Article 40 (March 2018), 28 pages. https://doi.org/10.1145/3131274

[6] A. K. Gottem, A. Sundaramoorthy, and A. Alagarsamy, “High Speed Approximate Carry Speculative Adder in Error Tolerance Applications”, IJC, vol. 21, no. 3, pp. 383-390, Sep. 2022.
