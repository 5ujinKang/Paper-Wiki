[Architectural Implications of Function-as-a-Service Computing | Proceedings of the 52nd Annual IEEE/ACM International Symposium on Microarchitecture](https://dl.acm.org/doi/abs/10.1145/3352460.3358296)

[Architectural Implications of Function-as-a-Service Computing (acm.org)](https://dl.acm.org/doi/pdf/10.1145/3352460.3358296)

전반적인 FaaS의 분석이었음. 속도가 느려지는 이유 등을 조사하고 실험해본 내용이고, cold starts의 정의와 그 이유를 일부 다루고 있지만 cold starts에 집중을 했다기 보다는 FaaS의 구조에 더 집중하고 있음. 

결과물로 낸 것도 어떤 쪽에서 시간이 많이 걸리는지를 “검사해주는” 툴을 제안.(openWhisk상에서)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/74c72bec-0123-4085-afa4-c660fa7d25b7/Untitled.png)

1. FaaS containerization이 오버헤드를 심화시킨다.(다른 cloud as a service들에 비해)
: FaaS containerization brings up to 20x slowdown compared to native execution, 
cold-start can be over 10x a short function’s execution time.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/35019690-8464-4fbf-9a52-7681123f457c/Untitled.png)

이유 : The FaaS provider is **unaware of functions’ applicationlevel performance metrics** beyond throughput and execution time.
As such, they do not guarantee metrics like 99th-percentile latency as in more controlled settings like PaaS or microservices. 

This exposes the developer to platform-level behavior like high cold start times.

Over-invoked 되었는지 등도 cold start에 영향을 미치는 것으로 보이나, OpenWhisk 내부상의 설정 문제일 가능성이 있음.

Over-invoked: If no container is available for the invoker
to provision to a function, the invocation is queued. 

When more functions are invoked than the server can execute (the
red 100ips test in Figure 4), latency keeps increasing, due to
the backlog of this internal queue. An active load balancer
can avoid sending new invocations to over-invoked servers.

(2) Under-invoked: OpenWhisk pauses idle containers after a
50ms grace period to save memory. 

On the other hand, if invocations are queued and the server has available resources,
new containers are started. 

Now, if a server is under-invoked (the blue 8ips test in Figure 4), containers become idle for
long enough to be paused. 

This causes a latency ripple effect as invocations must wait periodically for container(s) for functions to be unpaused.

(3) Balanced: If the invocation rate is high enough to avoid
pauses and low enough to stay within the server capacity,
function invocation latency converges. For the green 50ips
test in Figure 4, the latency converges to the execution time
of roughly 55ms after about 3.75 seconds
