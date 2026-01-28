# Background
Large-scale LLM training's failure diagnosis & handling practices are not effective.  
Fail-stop, diagnosis, and reassuming procedure incur too much overhead. even takes several hours to even days.

### Why is training a large-scale model unstable?
1. Implicit failures -> hard to detect and handle
2. Ultra-large training scale: not enough spare machines for recovery
3. Months-long LLM pretrain -> continuous evolution of user code w.ongoing integration of performance optimizations or algorithmic adjustment updates.

** Seems like there are 2 problem for the large-scale LLM training's failure handling  
** 1. Takes a bunch of time to handle the error  
** 2. Errors can occur frequently because it's unstable.  
**  
** then, can we solve 2nd? : I don't think so. unstableness is inherent.  
** Better focus on 1st. analyse why phase actually takes that much time.  

# Problem

# Prior Solution
1. implicit failures -> hard to detect and handle
   : timeouts & process termination to identify faulty machines.
   -> significant waste of GPU cycles.
3. 

# Goal
Efficient incident diagnosis & handling with minimal unproductive time.

# Key Idea
Extends LLM training job to leverage fine-grained process management.
-> capable of leveraging runtime information for failure detection and achieving fast recovery.

# Solution: High Level
1. Prioritize rapid isolation, not precise localization
   : precise failure pinpointing can leave vast GPUs idle
2. 

# Solution: Low Level


# Evaluation


# Interesting Points
- Optimization techniques and parallelization strategies derived from small-scale testing are often suboptimal at large scales.

# Good Points


# Bad Points
