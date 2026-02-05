- link : https://dl.acm.org/doi/epdf/10.1145/3731569.3764839

# Title
Sailor: Automating Distributed Training over Dynamic,  Heterogeneous, and Geo-distributed Cluster

# Problem
Current systems lack support for efficiently training models on heterogeneous resources.

# Background
- Allowing a training job to run on heterogeneous GPU types and/or GPUs distributed across zones (i.e., with heterogeneous inter-node bandwidth) can give model developers access to more GPUs per job to increase training throughput.
- However, supporting heterogeneous, geo-distributed resources introduces several challenges.

# Goal
- a system for efficient large-scale training over heterogeneous resources with dynamic availability.
- automates distributed training over heterogeneous, geo-distributed, and dynamically available resources.

# Challenges
1. Quickly searching a vast configuration space.
2. Accurately simulating memory footprint and iteration time.
3. Supporting both heterogeneous plans and seamless elasticity in a real distributed training framework.

# Priorwork's Limitation
1. Current works do not co-optimize the resource allocation with the job parallelization plan.
2. Existing systems rely on inaccurate simulators to estimate the training throughput and memory footprint of candidate configurations
3. state-of-the-art distributed training frameworks like Megatron-LM are slow to reconfigure jobs and do not support heterogeneous job parallelization plans or different microbatch sizes per GPU, which is necessary to maximize throughput and minimize cost in heterogeneous clusters.

# Design
- profiler: Collects information about the training job, the compute nodes and network bandwidth.
- configuration planner: navigates the search space of resource allocations and job parallelization plan combinations. It recommends configurations that optimize a user-defined objective (e.g., max throughput or min cost) under constraints (e.g., max budget or min throughput).
- simulator: accurately model iteration time and memory footprint for any given configuration.
- distributed training framework: adds support for heterogeneous configurations to execute the planner’s configurations. It also adds support for fault tolerance and elasticity, enabling adaptation to changes in resource availability.

# Solution
1. Current works do not co-optimize the resource allocation with the job parallelization plan: Do -> creates a vast search space -> 1) prunes the search space with heuristics that consider training memory footprint, GPU capacity, and scalability constraints. 2) applies dynamic programming to reuse information about the performance of parallelization plans.
2. Existing systems rely on inaccurate simulators to estimate the training throughput and memory footprint of candidate configurations: with more signals, with a certain formula
3. state-of-the-art distributed training frameworks like Megatron-LM are slow to reconfigure jobs and do not support heterogeneous job parallelization plans or different microbatch sizes per GPU, which is necessary to maximize throughput and minimize cost in heterogeneous clusters. : let the model work on Megatron

# Limitation
- Heterogeneity: May prevent the use of high-performance collective communication libraries
- Geo-distributed networks are prone to unreliability, including unpredictable jitter and packet loss.
- The optimization parallelization strategies are based on homogeneous settings -> maybe need to develop other strategies.
   
# Insight
Cons
- No insight
- lack of "why" of the prior work's limitation

Pros
- Smartly solved a problem in an ML situation using ML's Characteristics
  - predictable -> simulation  
  - Domain knowledge -> prune the search space
  - Only consider Throughput

- It's solid
  - Background: short + deliver minimal necessary information
  - Motivation: 3.1) gives 3 reasons / 3.2) Detailed survey of prior works, well-backed evidence - citations or numbers, Every piece of conclusion is supported by multiple evidence & arguments
  - 4.2.1) 6 techniques = Design is solid
  - Evaluation: lots of evaluation, lots of baselines
