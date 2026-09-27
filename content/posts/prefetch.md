+++
title = "Profit w/ Prefetching"
date = "2026-09-27"

[taxonomies]
tags=["microarch"]
+++


# Introduction

Modern processors use a critical microarchitectural technique known as **prefetching** to bridge the memory wall, i.e., the massive performance gap between ultra-fast CPUs and relatively slow main memory. Hardware prefetchers do their thing automatically in the background. <u> But software prefetching?</u> That's where we actually turn a profit. Because these instructions work on **virtual addresses**, we can literally use them to poke at memory and figure out if an address is **mapped or unmapped**. 


Take the x86 PREFETCHh instruction family (like [`prefetcht0`](https://www.felixcloutier.com/x86/prefetchh)) or the compiler's [`__builtin_prefetch`](https://man.netbsd.org/NetBSD-6.0/__builtin_prefetch.3). 
At their core, they take <u>one main parameter</u>: _a virtual address_ (a pointer). 


## Why Prefetch?

## Without Prefetching
When the CPU issues a load to a virtual address, it first checks the Translation Lookaside Buffer (TLB). On a TLB miss, the hardware is forced to perform a Page Table Walk (PTW). In standard x86-64 (4-level paging), this requires up to 4 separate memory reads (PML4 → PDPT → PD → PT) to translate the virtual address to a physical one. On modern CPUs with 5-level paging (LA57), this expands to 5 reads (PML5 → PML4 → PDPT → PD → PT). If those page table entries aren't already cached, each read hits main memory (DRAM), costing ~200+ cycles per level.

![TLB WALK](/tlb_walk.svg)
<figcaption style="font-size: 0.85em; color: #666; margin-top: 5px; text-align: center;">
    Figure 1: <a href='https://en.wikipedia.org/wiki/Intel_5-level_paging'>4-level paging of the 64-bit mode</a>
</figcaption>



## With prefetching
The data is pulled into the cache before the CPU actually asks for it. When the real load instruction finally executes, it hits the cache instantly. The pipeline stays full, and those hundreds of cycles of latency are completely hidden.

{% <character name="cpu" position="left" image="cpu.png"> %}I was about to stall for 4 DRAM trips on this virtual address... 😓{% </character> %}
{% <character name="hooded" position="right"> %}I knew you were gonna need this, so I already placed it into the cache. 😎{% </character> %}
{% <character name="cpu" position="left" image="cpu.png"> %}Thanks! That's a massive W for my execution pipeline.{% </character> %}


## The Segfault Trap

To understand why prefetching is such a massive profit, we have to look at what happens in assembly when we touch an unmapped address.

```asm,linenos,hl_lines=2
mov rdi, 0xdeadbeef ; invalid address
mov rax, [rdi]  ; Try to read the data
```
When the CPU executes this, the Memory Management Unit (MMU) tries to translate RDI. It fails. The CPU immediately halts execution and raises a Page Fault exception (#PF). The OS kernel steps in, sees the address is invalid, and sends your process a SIGSEGV (Segmentation Fault). Game over. Program crashes.


### The Safe Way

```asm,linenos,hl_lines=2
mov rdi, 0xdeadbeef ; invalid address
prefetcht2 [rdi]  ;  Ask the CPU to fetch it into level 3 cache and higher
```
The prefetch instruction is explicitly designed not to alter architectural state. If the address is unmapped or an invalid memory address, the CPU silently aborts the fetch. <u>No Page Fault. No Segmentation Fault. No crash.</u>

###  The Profit
This silent behavior is exactly how we build our **side-channel**. We use a technique like **Flush+Reload**. First, we flush an address from the cache. Then, we issue our silent prefetch. Finally, we reload the address and measure the time. If the <u>reload is fast</u>, the address was <u>mapped</u>. Why? Because prefetching didn't just warm up the data cache; it also populated the TLB, saving us from a costly page table walk. If it's slow, it wasn't prefetched. If unmapped, the prefetch silently ignores it, and we get our profit without a single crash.





# References

- [Prefetch Assembly Instruction](https://www.felixcloutier.com/x86/prefetchh)
- [Prefetch C function](https://man.netbsd.org/NetBSD-6.0/__builtin_prefetch.3)
- [Prefetch Intel SDM](https://intel.github.io/SDM/instruction/PREFETCHh.html)
- [Paging](https://os.phil-opp.com/paging-introduction/)
- [5 Level of Paging](https://en.wikipedia.org/wiki/Intel_5-level_paging)
- [MIT SHD ASLR BYPASS](https://shd.mit.edu/2026/labs/aslr.html#part-1b-prefetch-side-channels-40)