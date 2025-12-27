# Chapter 2: Memory Hierarchy & Queue Structures

## Overview

This chapter explains GPU memory types, their latencies, and why NVMe queue structures residing in global memory create performance bottlenecks.

---

## Memory Latency Pyramid

```
        ┌─────────────┐
        │  REGISTERS  │  ~1 cycle    (Per-thread)
        ├─────────────┤
        │   SHARED    │  ~5 cycles   (Per-block)
        │   MEMORY    │
        ├─────────────┤
        │  CONSTANT   │  ~5 cycles   (Per-grid, cached)
        │   MEMORY    │
        ├─────────────┤
        │   GLOBAL    │  ~500 cycles (Per-grid)
        │   MEMORY    │
        └─────────────┘
```

## Memory Characteristics

| Memory Type | Latency | Scope | Size | Read/Write |
|-------------|---------|-------|------|------------|
| Registers | ~1 cycle | Per-thread | ~256 KB/SM | R/W |
| Shared Memory | ~5 cycles | Per-block | 16-48 KB/SM | R/W |
| Constant Memory | ~5 cycles (cached) | Per-grid | 64 KB | Read-only |
| Global Memory | ~500 cycles | Per-grid | GBs | R/W |

---

## Bandwidth Calculation

```
Compute Power     = 1,000 GFLOP/s
Memory Bandwidth  = 150 GB/s
Bytes per FLOP    = 4 B
Achievable        = 150 GB/s ÷ 4 B/FLOP = 37.5 GFLOP/s = 3.75% utilization
```

---

## Key Takeaway

> NVMe queues live in Global Memory with ~500 cycle latency.
> Memory bandwidth alone limits GPU utilization to <4% for memory-bound operations.
