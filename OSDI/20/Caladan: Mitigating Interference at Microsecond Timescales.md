# 📄 Title : Caladan: Mitigating Interference at Microsecond Timescales
- **source:**   https://www.usenix.org/conference/osdi20/presentation/fried
- **Keywords:**  interference, microsecond
- **Core Idea (inferred from keywords):** 

Background
==========

Problem : latency increase due to interference
----------------------------------------------

1.  Minimizing tail latency is critical for services : end-to-end response times are determined by the slowest individual response
    
2.  large-scale data centers often pack several tasks together on the same machine : to improve CPU utilization-> tasks compete over shared resources (ex. Cores, memory bandwidth, caches, execution units)
    

\=> Interference gets high -> latency increases significantly

1.  Interference : slowdown of tasks due to shared resource contention
    

Solution : hardware mechanisms that partition resources
-------------------------------------------------------

Interferences that can occur when sharing a CPU
-----------------------------------------------

1.  Hyperthreading interference (in a physical core)
    
    1.  Can be serious when shared resources are contended
        
    2.  Hyperthreading : a single physical CPU core run two (or more) logical cores/threads.
        
2.  Memory bandwidth interference
    
    1.  Impact all cores that share the same physical CPU
        
3.  LLC interference
    
    1.  Impact all cores that share the same physical CPU
        

Problem
=======

*   Common Knowledge : CPU resources must be partitioned to achieve performance isolation between tasks.
    

### Attack Point

: Partitioning based systems can't react quickly to drastic changes (the amount & type of resources) in requests -> extreme spikes in latency & can't increase CPU utilizationsWhy? Partitioning mechanism

1.  Statically assign enough resources for peak load -> can't achieve good CPU utilization
    
2.  Or make dynamic adjustments : converging to the right configuration after a change in resource usage can take dozens of seconds.
    

Goal : faster reaction times
============================

Both Strict performance isolation & high CPU utilization, under frequently changing interference and load

Challeges
=========

1.  Sensitivity : many types of interference in a shared CPU (hyperthreading, memory bandwidth, LLC, etc) -> obtaining the right control signals that can accurately decect each of them is dificult
    
2.  Scalability : Existing systems face too much software overhead to either gather control signals or adjust resource allocations quickly
    

Solution : Caladan, interference aware CPU scheduler
====================================================

*   Instead of resource partitioning -> control signals & policies (rely on fast core allocation)
    

### How to Solve?

#### Sensitvity

1.  Carefully select control signals that enable fast detection of interference
    
2.  Dedicate a core : to monitor these signals & take action to mitigate interference as it arises
    
    1.  A centralized, dedicated scheduler core :
        
        *   collects control signals & makes resource allocation decisions
            
        *   Distinguishes between LC tasks and BE tasks
            
        *   Core allocation : avoid reaction time limitations imposed by hardware partitioning
            

#### Scheduler's Workflow

1.  Collect fine-grained measurements of (memory bandwidth usage & request processing times)
    
2.  Detect (memory bandwidth & hyperthreading interference)
    
3.  Adjust core allocations
    
4.  Restrict cores from BE tasks, eliminating most of the impact on service times
    

#### Scalability

1.  Linux Kernel module : KSCHED
    
    1.  Perform scheduling functions across many cores at once in a matter of microseconds
        
    2.  Goal : to efficiently expose control over CPU scheduling to the userspace scheduler core.
        
    3.  It runs on all cores managed by caladan & shifts scheduling work away from the scheduler cores to cores running tasks.
        
    4.  It leverages hardware support for multicast interprocessor interrupts (IPIs) to amortize the cost of initiating operations on many cores simultaneously
        
    5.  It provides a fully asynchronous scheduler interface so that the scheduler can initiate operations on remote cores and perform other work while waiting for them to complete
        

#### KSCHED's Workflow

: let Caladan react quickly while scaling to many cores & tasks, even under heavy interference

*   Amortize the cost of sending interrupts
    
*   Off-load scheduling work {from the scheduler core -> to tasks' core}
    
*   Provide a non-blocking API that allows the scheduler core to handle many inflight operations at once
    

Design
======

*   Top-level core allocator : to grant more cores to tasks that are experiencing queueing delays
    
*   The Memory Bandwidth Controller : to use the majority of available memory bandwidth while avoiding saturation
    
*   The Hyperthread Controller : detects hyperthread interference -> bans use of the sibling hyperthread until the current request completes
    
*   KSCHED : Fast and Scalable Scheduling



<br> <br>

## Insights: 
- core allocation can be a way to achieve performance isolation than resource partitioning
- make a tool to define the state (like using control signal to differentiate the interference) can be good way to deal with the tradeoff like problem



