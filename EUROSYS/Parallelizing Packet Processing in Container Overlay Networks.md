## 주제

Overlay Network ↔ physical Network 에서

throughput, latency 차이가 엄청남

⇒ understand the bottlenecks of in-kernel networking when
running container overlay networks

While overlay networks
are widely adopted in production systems, they cause significant performance degradation in both throughput and
latency compared to physical networks. This paper seeks to
understand the bottlenecks of in-kernel networking when
running container overlay networks

## 문제점

We perform a comprehensive study of the performance
of container overlay networks and identify the main
bottleneck to be the **serialization** of a large number of
softirqs on a single core.

## 해결방법

Falcon pipelines software interrupts associated with different network devices of a single flow on **multiple cores**, thereby **preventing execution serialization** of excessive software interrupts from overloading a single core.

## Falcon 핵심

Falcon centers on three designs: softirq pipelining, splitting, and dynamic balancing to enable fine-grained, low-cost flow parallelization on multicore machines.
