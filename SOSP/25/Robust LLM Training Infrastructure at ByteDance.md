# Background
Large-scale LLM training's failure diagnosis & handling practices are not effective.  
Fail-stop, diagnosis, and reassuming procedure incur too much overhead. even takes several hours to even days.

  
### Why is training a large-scale model unstable?
1. Implicit failures -> hard to detect and handle
2. Ultra-large training scale: not enough spare machines for recovery
3. Months-long LLM pretrain -> continuous evolution of user code w.ongoing integration of performance optimizations or algorithmic adjustment updates.
  
** Seems like there are 2 problems for the large-scale LLM training's failure handling  
** 1. Takes a bunch of time to handle the error  
** 2. Errors can occur frequently because it's unstable.  
**  
** Then, can we solve the 2nd? : I don't think so. Unstability is inherent.  
** Better focus on 1st. analyse why the phase actually takes that much time.  
  
# Problem
  
# Prior Solution
1. implicit failures -> hard to detect and handle  
   : timeouts & process termination to identify faulty machines.  
   -> significant waste of GPU cycles.  
3. 

# Goal
Efficient incident diagnosis & handling with minimal unproductive time.
  
# Key Idea
Extends LLM training job to leverage fine-grained process management. <br>
-> capable of leveraging runtime information for failure detection and achieving fast   recovery.
  
  
# Solution: High Level
1. Prioritize rapid isolation, not precise localization  
   : precise failure pinpointing can leave vast GPUs idle. <br>
   -> lightweight real-time detection with hierarchical stop-time diagnostics, quickly singling out faulty machines with minimal overhead: But this is not accurate<br>
   -> datadriven clustering of runtime stack-traces to isolate suspect machines, overevicting them rather than chasing exact root causes.<br>
     
2. Control variability during swift recovery
   - Automated fault tolerance framework + machine fault detection and diagnostics with code rollback for rapid verification and recovery.
   - User code changes are merged with deterministic failures through a lazy update approach, utilizing the inevitability and high frequency of failures.
  
3. Ensure controlled and rapid recovery:
   - leverage pre-provisioned warm standbys that execute self-checks before delivery to avoid full job rescheduling
   - checkpointing module -> backups & rapid restart
  
  
# Solution: Low Level


# Evaluation


# Interesting Points
- Optimization techniques and parallelization strategies derived from small-scale testing are often suboptimal at large scales.

# Good Points


# Bad Points
