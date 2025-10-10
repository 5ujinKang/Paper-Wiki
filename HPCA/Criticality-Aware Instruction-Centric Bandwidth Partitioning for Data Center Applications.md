📄 Title : Criticality-Aware Instruction-Centric Bandwidth Partitioning for Data Center Applications
source: https://zhou-diyu.github.io/files/pivot-hpca25.pdf
Keywords: interference, microsecond
Core Idea (inferred from keywords):
Background
Problem : latency increase due to interference
Minimizing tail latency is critical for services : end-to-end response times are determined by the slowest individual response

large-scale data centers often pack several tasks together on the same machine : to improve CPU utilization-> tasks compete over shared resources (ex. Cores, memory bandwidth, caches, execution units)

=> Interference gets high -> latency increases significantly

Interference : slowdown of tasks due to shared resource contention
Solution : hardware mechanisms that partition resources
Interferences that can occur when sharing a CPU
Hyperthreading interference (in a physical core)

Can be serious when shared resources are contended

Hyperthreading : a single physical CPU core run two (or more) logical cores/threads.

Memory bandwidth interference

Impact all cores that share the same physical CPU
LLC interference

Impact all cores that share the same physical CPU
Problem
Common Knowledge : CPU resources must be partitioned to achieve performance isolation between tasks.
Attack Point
: Partitioning based systems can't react quickly to drastic changes (the amount & type of resources) in requests -> extreme spikes in latency & can't increase CPU utilizationsWhy? Partitioning mechanism

Statically assign enough resources for peak load -> can't achieve good CPU utilization

Or make dynamic adjustments : converging to the right configuration after a change in resource usage can take dozens of seconds.

Goal : faster reaction times
Both Strict performance isolation & high CPU utilization, under frequently changing interference and load

Challeges
Sensitivity : many types of interference in a shared CPU (hyperthreading, memory bandwidth, LLC, etc) -> obtaining the right control signals that can accurately decect each of them is dificult

Scalability : Existing systems face too much software overhead to either gather control signals or adjust resource allocations quickly

Solution : Caladan, interference aware CPU scheduler
Instead of resource partitioning -> control signals & policies (rely on fast core allocation)
How to Solve?
Sensitvity
Carefully select control signals that enable fast detection of interference

Dedicate a core : to monitor these signals & take action to mitigate interference as it arises

A centralized, dedicated scheduler core :

collects control signals & makes resource allocation decisions

Distinguishes between LC tasks and BE tasks

Core allocation : avoid reaction time limitations imposed by hardware partitioning

Scheduler's Workflow
Collect fine-grained measurements of (memory bandwidth usage & request processing times)

Detect (memory bandwidth & hyperthreading interference)

Adjust core allocations

Restrict cores from BE tasks, eliminating most of the impact on service times

Scalability
Linux Kernel module : KSCHED

Perform scheduling functions across many cores at once in a matter of microseconds

Goal : to efficiently expose control over CPU scheduling to the userspace scheduler core.

It runs on all cores managed by caladan & shifts scheduling work away from the scheduler cores to cores running tasks.

It leverages hardware support for multicast interprocessor interrupts (IPIs) to amortize the cost of initiating operations on many cores simultaneously

It provides a fully asynchronous scheduler interface so that the scheduler can initiate operations on remote cores and perform other work while waiting for them to complete

KSCHED's Workflow
: let Caladan react quickly while scaling to many cores & tasks, even under heavy interference

Amortize the cost of sending interrupts

Off-load scheduling work {from the scheduler core -> to tasks' core}

Provide a non-blocking API that allows the scheduler core to handle many inflight operations at once

Design
Top-level core allocator : to grant more cores to tasks that are experiencing queueing delays

The Memory Bandwidth Controller : to use the majority of available memory bandwidth while avoiding saturation

The Hyperthread Controller : detects hyperthread interference -> bans use of the sibling hyperthread until the current request completes

KSCHED : Fast and Scalable Scheduling




Insights:
core allocation can be a way to achieve performance isolation than resource partitioning
make a tool to define the state (like using control signal to differentiate the interference) can be good way to deal with the tradeoff like problem
