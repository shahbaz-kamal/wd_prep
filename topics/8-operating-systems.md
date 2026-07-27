# 8. Operating Systems

**Process vs thread**
- Process: own address space, heavier, IPC needed to communicate.
- Thread: shares the process's memory/heap, own stack + registers, lighter context switch.

**Process states:** New → Ready → Running → Waiting/Blocked → Terminated

**Scheduling**
- FCFS — simple, convoy effect.
- SJF — optimal average wait, requires knowing burst time, risks starvation.
- Round Robin — time quantum, good for time-sharing, no starvation.
- Priority — starvation risk, fixed by aging.
- Preemptive vs non-preemptive: whether the OS can forcibly take the CPU back.

**Deadlock — four necessary conditions (all must hold)**
1. Mutual exclusion
2. Hold and wait
3. No preemption
4. Circular wait

Handling: prevention (break a condition), avoidance (Banker's algorithm), detection + recovery, or ignore (ostrich).

**Sync primitives**
- Mutex — locking, ownership, binary. Semaphore — signalling, counting, no ownership.
- Race condition — output depends on thread timing. Critical section — code needing exclusive access.
- Starvation — a process never gets resources. Livelock — processes change state but make no progress.

**Memory**
- Paging — fixed-size pages, causes **internal** fragmentation.
- Segmentation — variable-size logical segments, causes **external** fragmentation.
- Virtual memory — disk as memory extension. Page fault when a page isn't in RAM.
- Page replacement: FIFO (Belady's anomaly), LRU, Optimal (theoretical).
- Thrashing — excessive paging, CPU utilization collapses.
- TLB — cache for page-table lookups.
