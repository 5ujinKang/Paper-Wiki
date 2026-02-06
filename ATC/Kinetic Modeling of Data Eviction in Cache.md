# Title
Kinetic Modeling of Data Eviction in Cache
- link: https://www.usenix.org/conference/atc16/technical-sessions/presentation/hu

# Background
The reuse distance (LRU stack distance) is an essential  metric for performance prediction and optimization of  storage and CPU cache.

# Problem


# Prior Works Limitation
Counter Stacks and SHARDS: 
- good accuracy, time & space consumption is extremely low.
- BUT cannot characterize shared cache behavior through modeling individual programs ** why do we need this?


# Effect
enables fast measurement and low-cost sampling. 
It can produce the miss ratio curve (MRC) in linear time with extremely low space  costs.
** How?

# Kick

a new kinetic model for MRC  construction of LRU caches based on average eviction  time (AET). 
AET runs in linear time asymptotically and  uses sampling to minimize the space overhead.
- AET is a composable  metric: the MRC of a multi-programmed workload  in shared cache can be computed directly from the AET  of its member programs.
 
# Design

# Implementation

# Evaluation
- AET has the lowest level space and run-time overhead compared to past techniques, for both CPU workloads and storage workloads, while maintaining high  MRC accuracy.
- 
# New Knowledge
- Locality characterization techniques : We might can search with this keyword to figure out what is going on with Working Set Size estimation. 
- As for storage workloads, their sizes are usually much  larger than CPU workloads and their life span may last  for weeks or more.
- reuse distance: number of distinct data accesses between two consecutive accesses to the same location

# Insight
