*주의 : 2016년도 논문이라 시대에 뒤떨어질 수 있음*

- 요약 : Ingens 모델 제안
    
    Ingens is a framework for huge page support that relies on a handful of basic primitives to **provide transparent huge page support** in a principled, coordinated way.
    
    By managing **contiguity** as a first-class resource and by **tracking utilization and access frequency of memory pages**, Ingens is able to eliminate a number of fairness and performance pathologies that plague current systems.
    
    Ingens is a memory management redesign that brings
    performance, memory savings and fairness to memoryintensive applications with dynamic memory behavior.
    It is based on two principles: (1) memory contiguity is
    an explicit resource to be allocated across processes and
    (2) good information about spatial and temporal access
    patterns is essential to managing contiguity; it allows the
    OS to tell/predict when contiguity is/will be profitably
    used. 
    

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6dd2879f-4ffb-4d25-9f66-e656ab63d27a/Untitled.png)

- 배경
    - 기존 프로세서들은 TLB entry 수가 매우 작았지만 현재는 수천개의 huge page entry를 dual-level TBL로 제공
    - 기존 OS memory 관리에서는 4KB 페이지 기준으로 진행되는 방법이므로 (huge page hardware와 best-effort 알고리즘, spot fixes등) 시대에 뒤떨어짐 (DRAM Growth,)
    - 기존 OS 메모리 관리 방법에서 4KB기준으로 맞춰져 있기 때문에 리눅스, KVM등에서 large-memory work-loads를 지원한다고 해도 대부분의 workload들은 “unacceptable performance overheads, wasted memory capacity, and unfair performance variability”를 겪게됨

- 기존의 Huge page 사용에 있어서의 문제점과 개선방안
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e8f19f4c-2dc4-4fab-9f4a-dd2d7d011fc0/Untitled.png)
    

- 기존의 단점
    - fragmentation
        
        <aside>
        💡 but larger pages require larger regions of contiguous physical memory. 
        
        Large pages can suffer from internal fragmentation (unused portions within the unit of allocation) and can also increase
        
        external fragmentation (reducing the remaining supply of contiguous physical memory). 
        
        Using larger pages requires more active memory management from the system
        software to increase available contiguity and avoid fragmentation
        
        </aside>
        

- 실험한 워크로드
: 각 실험항목별로 따로 진행하는것 같긴 했음
[Ingens-osdi-final.key (usenix.org)](https://www.usenix.org/sites/default/files/conference/protected-files/osdi16_slides_kwon.pdf)
