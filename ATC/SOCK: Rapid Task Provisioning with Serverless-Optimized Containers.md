we analyze Linux container primitives, identifying **scalability bottlenecks related to storage and network isolation**. We also analyze Python applications
from GitHub and show that **importing many popular libraries adds about 100 ms to startup**. 

Based on these
findings, we implement SOCK, **a container system** optimized for serverless workloads. Careful avoidance of kernel scalability bottlenecks gives SOCK an 18× speedup
over Docker. 

A generalized-Zygote provisioning strategy
yields an additional 3× speedup. A more sophisticated
three-tier caching strategy based on Zygotes provides
a 45× speedup over SOCK without Zygotes. Relative
to AWS Lambda and OpenWhisk, OpenLambda with
SOCK reduces platform overheads by 2.8× and 5.3×
respectively in an image processing case study.

===================================

## 알아낸 것

we identify 1)container initialization and package dependencies as common causes of slow lambda startup

1)we uncover
scalability bottlenecks in the **network and mount namespaces** and **identify lighter-weight alternatives**. 

2)find that many popular packages take 100 ms to import,
and installing them can take seconds. 

Although the entire 1.5 TB package set is too large to keep in memory, we
find that 36% of imports are to just 0.02% of packages.

## 목표

Based on these findings, we implement SOCK (roughly
for serverless-optimized containers), a special-purpose
container system with two goals

: (1) low-latency invocation for Python handlers that import libraries and 

(2)

efficient sandbox initialization so that individual workers
can achieve high steady-state throughput. We integrate
SOCK with the OpenLambda [20] serverless platform,
replacing Docker as the primary sandboxing mechanism.

## 기술

SOCK is based on three novel techniques. 

First, SOCK uses **lightweight isolation primitives**, avoiding the
performance bottlenecks identified in our Linux primitive study, to achieve an 18× speedup over Docker. 

Second, SOCK **provisions Python handlers** using a generalized Zygote-provisioning strategy to avoid the Python
initialization costs identified in our package study. In
the simplest scenarios, this technique provides an additional 3× speedup by avoiding repeated initialization
of the Python runtime. 

Third, we leverage our generalized Zygote mechanism to build a **three-tiered packageaware caching system**, achieving 45× speedups relative to SOCK containers without Zygote initialization.

 In an image-resizing case study, SOCK reduces cold-start
platform overheads by 2.8× and 5.3× relative to AWS
Lambda and OpenWhisk, respectively.

## 해결방법

First, we build a **lean container** system for sandboxing lambdas (§4.1). Second, we **generalize Zygote provisioning** to scale to large sets of untrusted packages (§4.2).
Third, we design **a three-layer caching system** for reducing package install and import costs (§4.3).
