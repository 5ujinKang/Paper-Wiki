## 주제 요약

: FaaS 특성 정리, cold start overhead를 줄일수 있는 방법 제시

## Managing cold-start invocations.

Using observations from
our characterization, we also propose a practical resource management policy for reducing the number of cold start executions while consuming no more resources than the large cloud providers’ current policies. 

Specifically, AWS and Azure use
a fixed “keep-alive” policy that retains the resources in memory for 10 and 20 minutes after a function execution, respectively [39, 40]. Though this policy is simple and practical,
it disregards the functions’ actual invocation frequency and
patterns, and thus behaves poorly and wastes resources.

In contrast, our policy (1) uses a different keep-alive value
for each user’s workload, according to its actual invocation
frequency and pattern; and (2) enables the provider in many
cases to pre-warm a function execution just before its invocation happens (making it a warm start). 

Our policy leverages a
small histogram that keeps track of the recent function interinvocation times. For workloads that exhibit clear invocation
patterns, the histogram makes clear how much keep-alive is
beneficial and when the pre-warming should take place.
