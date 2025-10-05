my presenation slides : https://docs.google.com/presentation/d/1d3wfehxgUbBJhlxRJfxRy3I7JtcGpTACrCyT37dZvwU/edit

## Snapshotting : 기존 산업에서 cold start overhead를 해결하기 위해 사용하는 방법

To reduce
cold-start latency, the industry has turned to snapshotting, whereby
an image of a fully-booted function is stored on disk, enabling a
faster invocation compared to booting a function from scratch.
This work introduces vHive, an open-source framework for
serverless experimentation with the goal of enabling researchers
to study and innovate across the entire serverless stack.

하지만 실험해보니 이것도 memory-resident 환경에서 실행시킬때에비해 실행시간이 95%나 더 길다는걸 알게됨

We find
that the execution time of a function started from a snapshot is
95% higher, on average, than when the same function is memoryresident

이러한 지연의 원인은 잦은 page faults 때문임을 밝혀냄

We show that the high latency is attributable to frequent
page faults as the function’s state is brought from disk into guest
memory one page at a time.

같은 function은 서로 다른 invocation들 사이에서도 같은,안정적인 working set에 접근 가능하다는 것을 알아냄

our analysis further reveals that
functions access the same stable working set of pages across different invocations of the same function.

첫 실행때, 디스크에서 working set을 메모리로 prefetch ⇒ page faults를 줄이겠다. = REAP

we build REAP, a light-weight software mechanism for serverless hosts
that records functions’ stable working set of guest memory pages
and proactively prefetches it from disk into memory.

. Our evaluation shows that
REAP eliminates 97% of the pages faults, on average, and reduces
the cold-start latency of serverless functions by an average of 3.7×

**REAP: RECORD-AND-PREFETCH**
The compact and stable working set of a function’s guest memory
pages, which instances of the given function access across invocations of the function, provides an excellent opportunity to slash
cold-start delays by prefetching.
Based on this insight, we introduce Record-and-Prefetch (REAP),
a light-weight software mechanism inside the vHive-CRI orchestrator to accelerate function invocation times in serverless infrastructures. REAP records a function’s working set upon the first
invocation of a function from a snapshot and replays the record to
accelerate load times of subsequent cold invocations of the function
by eliminating the majority of guest memory page faults. The rest
of the section details the design of REAP.

### 실험환경 : vHive를 사용하여 실험

- First, we adopt a number of functions, listed in **Table 1**, from a representative serverless benchmark suite called FunctionBench.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b12691e3-a268-46a3-96ce-a1cb2c57e915/Untitled.png)

- Second, to simulate the low invocation frequency of serverless functions in production,
the **host OS’ page cache is flushed before each invocation of a cold function**.
- To evaluate the cold-start start delay in a serverless platform **similar to AWS Lambda**, 
we augment the **vHive-CRI orchestrator** to act as a MicroManager in AWS.
- To study the memory access patterns of serverless functions,
we **trace the guest memory addresses** that a function instance accesses between the point when the vHive-CRI orchestrator starts to load a VM from a snapshot and the moment when the orchestrator receives a response from the function.
- As Firecracker lazily populates the guest memory, first memory access to each page from
the hypervisor or the guest raises a page fault in the host that can be traced. 
We use Linux **userfaultfd** feature that allows a userspace process to inspect the addresses and serve the page faults on behalf of the host OS.
