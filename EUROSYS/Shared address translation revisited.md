[Shared address translation revisited | Proceedings of the Eleventh European Conference on Computer Systems (acm.org)](https://dl.acm.org/doi/abs/10.1145/2901318.2901327)

[2901318.2901327 (acm.org)](https://dl.acm.org/doi/pdf/10.1145/2901318.2901327)

# Shared Address Translation Revisited

### 문제상황

Modern operating systems avoid duplication of code and data when they are mapped by multiple processes by sharing physical memory through mechanisms like copy-on-write.

Nonetheless, **a separate copy of the virtual address translation structures, such as page tables, are still maintained for each process, even if they are identical**. 

This duplication can lead to **inefficiencies in the address translation process and interference within the memory hierarchy.** 

### 목적

In this paper, we show that on Android platforms, **sharing address translation structures**, specifically, page tables and TLB entries, for shared libraries **can improve performance**. 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a5c6ee04-1e7f-49eb-b99e-15d0a55267ac/Untitled.png)

### 어떻게?

When a **page fault on a read access occurs** for the first time on any process for a page belonging to a shared PTP, **the corresponding PTE in the shared PTP is populated**.

The new PTE is then visible to all sharers, thereby **avoiding additional soft page faults** on read access to the same page.

To share a PTP, we first check whether the NEED_COPY bit of the level 1 PTE is set. 

(1) If NEED_COPY is not set,
in order to prevent modification, we must first **write-protect
every PTE with write permission** in the target PTP before
we can share it. 

When all memory regions in the range of the level 1 PTE have been traversed and write protected, 

we mark the PTP **as shared** and increment the sharer count for the PTP. 

Finally, we populate the corresponding level 1 PTE
of the child process with a pointer to the shared PTP. 

(2) If NEED_COPY is set, 

this means the corresponding PTP **is already shared** and its PTEs are still write protected. 

In this case, we only need to populate the child’s level 1 PTE with
a pointer to this shared PTP and increment the PTP’s sharer
count.

### 평가

For example, at a low level, sharing address translation structures **reduces the cost of fork** by more than half by **reducing page table** construction overheads. 

At a higher level, **application launch and IPC are faster** due to page fault elimination coupled with
better cache and TLB performance when context switching.

The **cost of fork** is reduced to **less than half** that of the stock kernel when using our shared address translation infrastructure. 

At the same time, **steady state performance** is also improved: the number of page faults for file-based mappings is reduced by 38% and the number of PTPs allocated by 35% respectively. 

**Page fault elimination coupled** with better cache performance results in a 10% improvement in
performance for an Android application launch.
