- link : https://dl.acm.org/doi/10.1145/3731569.3764812
- Topic : Make PCIe pooling usable

## Background
- PCIe devices(NICs, SSD) are frequently underutilized in cloud platforms: allocated conservatively to satisfy the peak demand of workloads.
- PCIe device pools can solve PCIe underutilization problem : multiple hosts share devices together.
- PCIe device pools : PCIe switches or Disaggregating PCIe devices over RDMA.
  
## Problem
- PCIe switches: expensive & inflexible(each type of PCIe switch has limited support).
- Disaggregating PCIe devices over RDMA: Can't separate other PCIe devices such as NICs.

## KeyIdea
Oasis: a system that enables pooling PCIe devices in software.
- Allows hosts to access PCIe devices located at any host within a CXL pod.
- CXL memory pools : can be used as shared memory across all the hosts in a CXL pod
  - mitigate stranded memory, improve memory utilization, and scale up in-memory databases
- CXL already works well with memory -> use this for PCIe device pooling at near-zero extra cost.

## Challenges
- CXL memory pool devices are not cache-coherent across hosts. -> to make it work as cache-coherent way
  1. Solve the overhead from cache line invalidations & memory fences
  2. Requires an efficient message channel over shared CXL memory = enable prefetch

## Design
goal: reuse existing CXL pod designs
1. Datapath: to enable an instance to communicate with PCIe devices located at a different host.
    - Allows I/O buffers, signaling message channels to be allocated in the shared CXL memory -> can be accessed by any hosts within a CXL pod directly.
    - I/O buffers : PCIe devices (e.g., NICs and SSDs) typically access I/O buffers directly via DMA (bypass CPU caches for I/O buffers)
    - Message channels :  modify the receiver to invalidate a cache line once all messages in it have been consumed -> prefetching always bring in new messages.,remove premature prefetching (stale cache lines)
2. Centralized control plane = pod-wide allocator : maps devices - hosts / load balancing / failure mitigation

## Implementation
1. Oasis Engine : Frontend Driver <-communicate over datapath-> Backend driver(PCIe device is here)
2. Control Plane : pod-wide allocator

## Evaluation 
Oasis improves the NIC utilization by 2× and handles NIC failover with only a 38 ms interruption.

## Limitation
1. CXL failures : the most common CXL related failure arises from CXL link/cable faults. -> should have resilient CXL pod designs.
2. QoS control for CXL bandwidth : What if CXL bandwidth is saturated? -> may rebalance traffic (ex. with intelRDT) 

## Meeting
- Completeness : Design Complete / Read lots of paper
- Limitation : work with Cloud. Not data center -> Why can't work with data center?
  1. DDIO, 2. zero-copy technique


