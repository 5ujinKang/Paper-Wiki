# Background
Large-scale LLM training's failure diagnosis & handling practices are not effective.
Fail-stop, diagnosis, and reassuming procedure incur too much overhead. even takes several hours to even days.

### Why is training a large-scale model unstable?
1. Implicit failures -> hard to detect and handle
2. Ultra-large training scale: not enough spare machines for recovery
3. Months-long LLM pretrain -> continuous evolution of user code w.ongoing integration of performance optimizations or algorithmic adjustment updates.

# Problem

# Prior Solution
1. implicit failures -> hard to detect and handle
   : timeouts & process termination to identify faulty machines.
   -> significant waste of GPU cycles.
3. 

# Goal
Efficient incident diagnosis & handling with minimal unproductive time.

# Key Idea


# Solution: High Level


# Solution: Low Level


# Evaluation


# Interesting Points
- Optimization techniques and parallelization strategies derived from small-scale testing are often suboptimal at large scales.

# Good Points


# Bad Points
