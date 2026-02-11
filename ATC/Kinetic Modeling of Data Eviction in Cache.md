# Title
Kinetic Modeling of Data Eviction in Cache
- link: https://www.usenix.org/conference/atc16/technical-sessions/presentation/hu
![49e30662e2a02d642643106b3fc50f47](https://github.com/user-attachments/assets/fc275f82-bce4-490a-b2e3-7244eec72c38)

# Keywords
Miss Ratio Curves (MRC), Average Eviction Time (AET), Shared Cache

# Summary
Using average  eviction time (AET) measured by sampling, the AET  model consumes liner time and extremely low space  for MRC profiling.

# Background
MRC is needed for the program's locality characterization.

# Problem
Prior work cannot characterize shared cache behavior by modeling individual programs.

# Prior Works Limitation
Counter Stacks and SHARDS: 
- good accuracy, time & space consumption is extremely low.
- BUT cannot characterize shared cache behavior through modeling individual programs

# Kick
1. Use average eviction  time (AET). AET is composable,  and it can drastically reduce the time and space overhead  of shared-cache optimization.
"Block moves its position whenever the reuse time is greater than the arrival time."
2. Use Reuse Time HistogrSampling -> for efficiency.

# Effect
enables fast measurement and low-cost sampling. 
- AET runs in linear time asymptotically and  uses sampling to minimize the space overhead.
- AET is a composable  metric: the MRC of a multi-programmed workload in shared cache can be computed directly from the AET  of its member programs.
It can produce the miss ratio curve (MRC) in linear time with extremely low space  costs.

 
# Design
- The miss ratio mr(c) at cache size c is the probability that a reuse time is greater than the average eviction time AET.
- We define the reuse time of every cold miss to be infinite.
- For efficiency, AET-based MRC profiling can use sampled RTH instead of real RTH. AET uses random and reservoir  sampling to reduce space cost in practice.

# Implementation
- Reservoir sampling: reduce the space complexity of RTH sampling from O(M) to O(1).
- Phase sampling: For programs that have an unstable reference pattern, we  evenly divide its execution into phases. The miss ratio at any cache size is the average miss ratio of all MRCs at this size.


# Evaluation
- AET has the lowest level space and run-time overhead compared to past techniques, for both CPU workloads and storage workloads, while maintaining high  MRC accuracy.
- AET show much higher throughput (arithmetic average) and  much lower space overhead (weighted average) than both  methods of Counter Stacks.
- the actual memory usage of AET random sampling  is much lower than Counter Stacks.
- It takes Counter Stacks O(log M) time to update the  counters at each reference and O(N log M) for the entire trace. AET is linear time in O(N).
- AET  makes a statistical assumption, offering good accuracy in  most cases in O(N) time. Counter Stacks makes no statistical assumption, delivering good accuracy in all cases  in O(N log M) time.
- composability, which can be used to model shared cache. But this is not a property of  current SHARDS and Counter Stacks.
- The average accuracy  improvement of full-trace AET against StatStack in this  test is 35.8%.
 -  StarStack assumes that every  reference will have a next reuse. The number of  references with no reuse is the same as the working set  size. The accuracy of their model is thus influenced by  the ratio of the working set size to the trace length.
 -  In most  benchmarks, phase AET sampling is more accurate than  non-phase AET sampling.


# New Knowledge
- Locality characterization techniques : We might can search with this keyword to figure out what is going on with Working Set Size estimation. 
- As for storage workloads, their sizes are usually much  larger than CPU workloads and their life span may last  for weeks or more.
- reuse distance: number of distinct data accesses between two consecutive accesses to the same location
- Cache sharing means that all co-run programs have  the same average eviction time (AET).
- StatStack is one of the most efficient and accurate methods to approximate MRC for CPU workloads.

  
# Insight
- To estimate cache usage, we don't need the exact location(stack location) of the accessed data. We can use relative location (reuse time).
- Block moves its position whenever the reuse time is greater than the arrival time.
- Trade off: AET  makes a statistical assumption, offering good accuracy in  most cases in O(N) time. Counter Stacks makes no statistical assumption, delivering good accuracy in all cases  in O(N log M) time.
- Cover both: The AET models the average eviction time,  so it is more accurate when a program shows a steady access behavior. If a program has different phase behavior,  we should apply AET analysis on each phase separately  as we have done here.
- ABF Can be a good way too, if we want just simple, cheap way.
 -  The footprint-based MRC profiling technique needs  recording every access during the monitoring window.
 -  ABF approximates the footprint of the  entire program by the footprint of small portions. The  length of a sampled trace (a burst interval) is bounded by  the cache size and minimal miss ratio of interest.
 -  ABF sampling has several limitations. : no sampling, ABF sample is a consecutive portion of a trace.  Its result is accurate only if the locality of burst intervals  is the same as the locality of the rest
 -  The MRCs of ABF sampling are much shorter than AET sampling, because using bursty interval to represent the entire trace will lose  the reference pattern in the hibernation interval. The approximate MRCs of ABF sampling are not as accurate as  AET sampling.
 -  Several studies  use on-line MRC analysis for cache partitioning
  -  Xiao Zhang, Sandhya Dwarkadas, and Kai Shen. Towards practical page coloring-based multicore cache management. In Proceedings of the 4th ACM European con
  -  G Edward Suh, Srinivas Devadas, and Larry Rudolph. Analytical  cache models with applications to cache partitioning.
