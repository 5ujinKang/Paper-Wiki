[MEGA: Overcoming Traditional Problems with OS Huge Page Management (acm.org)](https://dl.acm.org/doi/pdf/10.1145/3319647.3325839?casa_token=gFbH49qvYEEAAAAA:Jw8warRQR_0ylBtxcGQXTCuWcfj3pXNqaI1BfQsFbKIZ8HrSNgbvQVWF_cBmd9ICmUTlpMQHnGtz)

Notes : https://drive.google.com/file/d/1YyuJ4bBwforSioCmExQxHOW0fptuYcOC/view?usp=sharing

### ABSTRACT

- demonstrate the benefits and drawbacks of using
such huge page.
- that it is essential
for modern OS to refine their software mechanisms to more
effectively manage huge pages.

### Huge Page 장점

- TLB HIT rate up → reducing performance overhead
    - **TLB misses** account for up to **50%** of the total execution time.
    By grouping multiple 4KB pages into one 2MB page, TLB misses are reduced, thus reducing this performance overhead
- efficient address translaton (in Linux)
    - in Linux, an address translation requires traversing 5-level page tables. With the use of huge pages, TLB misses are less expensive, since address translations stop at the **4th level.**

### Huge Page 단점

- incresed page fault latency
    - 4KB → 2MB로 할당해야할 공간이 늘어나면서 메모리에서도 2MB만큼 연속적인 메모리공간을 찾아야함
    → fragmented된 공간을 압축하는 작업이 필요
    - due to security reasons, the kernel has to zero out every page before giving them to processes, and a huge page needs more time to be zeroed.
- memory bloating
    - The Linux kernel allocates a huge page
    on every base page fault.
    - 실제 프로세스가 필요한 페이지보다 더 큰 메모리를 할당
    - memory footprint 증가
- Fragmentation (internal / external)
    - internal : 
    when a chunk of memory is not fully utilized.
    - external :
        
        External fragmentation occurs when free
        physical memory is divided in smalls chunks
        
- Huge pages are not swappable or migratable
    
    : The current kernel implementation does not support huge-page swapping. 
    This means that a huge page has to be demoted
    to base pages, before swapping out these pages. 
    
    This process degrades performance by delaying swapping and invalidating the corresponding TLB entry.
