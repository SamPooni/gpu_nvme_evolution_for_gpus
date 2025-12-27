# Chapter 4: CPU vs GPU I/O Workload Comparison

## Overview

This chapter compares how CPUs and GPUs approach storage I/O.

---

## Scale Comparison

| System | Threads | Queue Model |
|--------|---------|-------------|
| CPU | ~100 cores | 1 thread per queue (dedicated) |
| GPU (H200) | 270,336 | O(100,000) threads per queue (shared) |

## Performance Metrics

| Metric | CPU Workload | GPU Workload | Ratio |
|--------|--------------|--------------|-------|
| Throughput | 500 MB/s - 1 GB/s | 3 - 14+ GB/s | 3-14x |
| IOPS Target | ~1 Million | ~100 Million | 100x |
| Threads/Queue | 1 (dedicated) | O(100,000) (shared) | 100,000x |

---

## Why NVMe Works for CPUs

1. Dedicated queue per core - No contention
2. Interrupt-driven completion
3. O(100) queues sufficient
4. Serial access assumed

## Why NVMe Fails for GPUs

1. Shared queues - Thousands of threads compete
2. No interrupt support
3. O(100,000) contexts needed
4. Parallel access required but protocol forces serialization

---

## Key Takeaway

> CPUs need ~100 queues for ~100 cores (1:1 ratio).
> GPUs need queues for ~300,000 threads but get the same ~100 queues.
> This 3,000:1 thread-to-queue ratio creates the bottleneck.
