문제점 : FaaS가 provisioning을 위한 비용이 아주 많이 든다. 이것을 해결하겠다. ⇒ FaasNet

This indicates that the cost of
image pull dominates most functions’ cold start costs.

컨테이너 이미지 사이즈가 큰 컨테이너를 pulling 할 경우 cold starts overhead가 심화됨.

기존 해결법에서는 P2P를 사용하여 이를 해결했지만, 그러기 위해선 하나의 강력한 서버가 필요

이런 조건을 FaaS 인프라에서 충족시키기 어렵기 때문에 제안한 것이 FaaSNet (by decentralizing the container provisioning process across host VMs that are organized in function-based tree
structures)

**. FAASNET
enables high adaptivity via a tree balancing algorithm that
dynamically adapts the FT topology in order to accommodate
VM joining and leaving.**

Pulling large container images from
the backing store would incur significant cold startup latency,
which can be up to several minutes (§2.2.2) if the backing
store is under high contention.
Existing solutions cannot be directly applied to our FaaS
platform. Solutions such as Kraken [16], DADI [42], and
Dragonfly [10] use peer-to-peer (P2P) approaches to accelerate container provisioning at scale; however, they require one
or multiple dedicated, powerful servers serving as root nodes
for data seeding, metadata management, and coordination.
Directly applying these P2P-based approaches to our existing
FaaS infrastructure is not an ideal solution due to the following reasons. (1) It would require extra, dedicated, centralized
components, thus increasing the total cost of ownership (TCO)
for the provider while introducing a performance bottleneck.
(2) Our FaaS infrastructure uses a dynamic pool of resourceconstrained VMs to host containerized cloud functions for
strong isolation; a host VM may join and leave the pool at
any time. This dynamicity requires a highly adaptive solution,
which existing solutions fail to support

To address these challenges, we present FAASNET, a
lightweight and adaptive middleware system for accelerating
serverless container provisioning. FAASNET enables high
scalability by decentralizing the container provisioning process across host VMs that are organized in function-based tree
structures. A function tree (FT) is a logical, tree-based network overlay. A FT consists of multiple host VMs and allows
provisioning of container runtimes or code packages to be
decentralized across all VMs in a scalable manner. FAASNET
enables high adaptivity via a tree balancing algorithm that
dynamically adapts the FT topology in order to accommodate
VM joining and leaving
