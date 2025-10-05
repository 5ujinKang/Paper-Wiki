[SEUSS | Proceedings of the Fifteenth European Conference on Computer Systems (acm.org)](https://dl.acm.org/doi/abs/10.1145/3342195.3392698)

PDF : [SEUSS: Skip Redundant Paths to Make Serverless Fast (acm.org)](https://dl.acm.org/doi/pdf/10.1145/3342195.3392698)

# SEUSS: Skip Redundant Paths to Make Serverless Fast

### 제안모델 요약

This paper presents a system-level method for achieving the rapid deployment and high-density caching of serverless functions in a FaaS environment. 

For reduced start times**, functions are deployed from unikernel snapshots, bypassing expensive initialization steps.** 

- In SEUSS, unikernel snapshots speed up invocations by
deploying functions within fully-initialized environments.
- In SEUSS, function logic is packed with a language interpreter and library OS into an isolated unikernel.
- The **flat address space** of the unikernel enables a straightforward method for **capturing and caching the entire memory footprint and register state of the function**.
- Function state can be captured in a black-box fashion at an arbitrary point during
execution, as an in-memory **snapshot** image.
- New execution can be rapidly deployed from a snapshot, **dramatically shortening** the function’s start time by skipping the following paths: booting the unikernel, initializing the language runtime, and importing and compiling the function code and dependencies.
- Through our method, anticipatory optimizations are achieved
by **preemptively warming the internal pathways and data
structures of the unikernel stack** prior to capturing a snapshot image.

To reduce the memory footprint of snapshots we apply **page-level sharing across
the entire software stack** that is required to run a function.

- enable page-level sharing across snapshot stacks and UCs.
- By applying **page sharing across the entire software stack**,
the memory footprint of snapshots are considerably smaller
than processes, containers or microVMs, enabling a greater
number of function instances to be cached on a node.

1. unikernel → snapshot
    1. unikernel을 사용해서 **capturing and caching the entire memory footprint and register state of the function을 더 쉽도록.**
    2. snapshot을 사용해서 실행단계를 줄임

1. page sharing → memory footprint 줄여서 사용

### 동작

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e5f72c72-7e11-4fdc-b6dc-b1c07848f937/Untitled.png)

- B : unikernel is booted
- C : UC (Unikernel Contexts)
- S : a function-specific snapshot
- W : Warm
- H : Hot

### 평가

We demonstrate the effects of our techniques by replacing Linux on the compute node of a FaaS platform architecture.
With our prototype OS, the deployment time of a function 

**drops from 100s of milliseconds to under 10 ms.** 

**Platform throughput improves by 51x** on workload composed entirely of new functions. 

We are able to cache over **50, 000 function** instances in memory as opposed to 3, 000 using standard OS techniques. 

In combination, these improvements give the FaaS platform a new ability to handle large-scale bursts of requests.
