# Chapter 7: Synchronization Tax Calculation

## Overview

Quantifying the performance impact of NVMe synchronization on GPU utilization.

---

## The Calculation

```
Total GPU Threads    = 132 SMs × 2,048 threads = 270,336
Threads per Queue    = 270,336 ÷ 64 queues = 4,224
Warps per Queue      = 4,224 ÷ 32 = 132

During Serialization:
  Active threads/warp = 1
  SM Utilization      = 1 ÷ 32 = 3.1%
  Wasted Capacity     = 96.9%
```

---

## Four Sources of Overhead

1. **SM Cycle Loss** - 1 of 32 threads active
2. **L1 Cache Impact** - Atomic ops consume bandwidth
3. **Rescheduling** - Thread yield/reload per sync point
4. **Memory Overhead** - 16×QP_size + 256KB per SSD

---

## Key Takeaway

> During NVMe synchronization, GPUs waste 96.9% of their compute capacity.
> This is a fundamental protocol mismatch, not a software bug.
