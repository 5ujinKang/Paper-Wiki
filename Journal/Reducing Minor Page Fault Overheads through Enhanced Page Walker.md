https://dl.acm.org/doi/10.1145/3547142

## Reducing Minor Page Fault Overheads through Enhanced Page Walker

- Minor Page Fault
    
    요청한 페이지가 물리 메모리에는 로드되었지만 메모리 관리 유닛(Memory Management Unit, MMU)에는 로드되어있지 않다고 표시된 경우 이를 Minor 페이지 폴트라 한다.  
    
    어느 프로세스의 한 스레드가 어떤 페이지를 사용하고 있을 때 해당 프로세스의 다른 스레드가 그 페이지를 요청하는 경우를 예시로 들 수 있다.
    
    이 경우, MMU는 해당 스레드에서 요청한 페이지가 이미 사용되고 있는 페이지를 가리키도록 하면 되기 때문에 swap in이 발생하지 않는다. (다시말해 Disk I/O가 생기지 않는다)
    

### 배경

Application virtual memory footprints are growing rapidly in all systems 

from servers down to smartphones.

To address this growing demand, system integrators are incorporating ever larger amounts of main memory, warranting rethinking of memory management. 

In current systems, applications produce page fault exceptions 

whenever they access virtual memory regions which are not backed by a physical page. 

### 문제상황

As application memory footprints grow, they induce more and more minor page faults. 

Handling of each minor page fault can take few 1000’s of CPU-cycles and blocks the application till OS kernel finds a free physical frame. 

These page faults can be detrimental to the performance, when their frequency of occurrence is high and spread across application run-time. Specifically, lazy allocation induced minor page faults are increasingly impacting application performance. 

Our evaluation of several workloads indicates an overhead due to minor page faults
as high as 29% of execution time.

### 목적 및 제안내용

In this paper, we propose to mitigate this problem through a hardware, software co-design approach.

Specifically we first propose to parallelize portions of the kernel page allocation to run ahead of fault time in a **separate thread**. 

Then we propose the Minor Fault Offload Engine(MFOE), a per-core hardware accelerator for
minor fault handling. MFOE is equipped with pre-allocated page frame table that it uses to service a page fault. 

On a page fault, MFOE quickly picks a pre-allocated page frame from this table, makes an entry for it
in the TLB, and updates the page table entry to satisfy the page fault. The pre-allocation frame tables are periodically refreshed by a background kernel thread, which also updates the data structures in the kernel to account for the handled page faults. 

상세)

1. **kernel page allocation 일부를 병렬화 ⇒ 분리된 스레드에서 실행하도록하여 실행시간 줄이기**
    1. All the applications are run in **multi-threaded mode**(if possible) and parallel fault
    events are taken into consideration, when calculating minor fault overhead.
    2. MFOE parallelizes portions of the kernel page allocation to run ahead of fault time in a separate thread.
2. **MFOE : pre-allocated page frame ⇒ page fault 발생시 바로 사용할 수 있도록**
    
    We have broken down the page fault handling in software into 3 parts- tasks 
    
    that can be done **before** the page fault, 
    
    tasks to be done **for handling the page fault(critical path)** and 
    
    tasks that can be done **after** the page fault. 
    
    Design of MFOE is primarily motivated by two main observations pertaining to these sub-tasks.
    
    (1) Tasks that can be done **before and after** the page fault occurrence, can **be removed from the critical path to reduce the fault handling latency.** 
    
    (2) The critical path tasks are hardware amenable, hence would **benefit if the hardware(MFOE) executes the critical path** instead of software.
    

**Pre-fault and post-fault functions are combined into a background thread, so their latencies are removed from the fault handing function in the context of the faulting program.**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fb2f04bf-83b8-4ac8-b60a-e4b127fb0442/Untitled.png)

### 평가

We evaluate this system in the gem5 architectural simulator with a modified Linux kernel running on top of simulated hardware containing the MFOE accelerator. 

Our results show that MFOE improves the average critical-path fault handling latency by 33× and tail critical path latency by 51×. 

Amongst the evaluated applications, we observed an improvement of run-time by an average of 6.6%.
