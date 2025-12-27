# Chapter 1: GPU Architecture Fundamentals

## Overview

This chapter explains how GPU threading works, which is essential to understanding why NVMe's queue design creates bottlenecks.

---

## Key Concepts

### 1. Thread Hierarchy (5 Levels)

```
Level 1: GRID
    └── Level 2: BLOCK (Thread Block)
            └── Level 3: WARP (32 threads)
                    └── Level 4: THREAD
                            └── Level 5: Assigned to SM (Streaming Multiprocessor)
```

### 2. SIMT Architecture

**Single Instruction, Multiple Thread (SIMT)**:
- All 32 threads in a warp execute the **same instruction** simultaneously
- Efficient when threads perform identical operations
- **Problem**: When threads must serialize (e.g., NVMe queue access), only 1 of 32 threads is active

### 3. Critical Specifications

| Parameter | Value | Why It Matters |
|-----------|-------|----------------|
| Warp Size | 32 threads | Serialization wastes 31 threads |
| Max Block Size | 1,024 threads | Limits parallelism granularity |
| Max Blocks/SM | 32 (Maxwell) | Resource allocation limit |
| Max Threads/SM | 2,048 (Maxwell) | Total concurrency per SM |

---

## The Problem for NVMe

When GPU threads access NVMe queues:

1. Multiple threads from different warps may access the same queue
2. NVMe requires serialized operations (doorbell writes, command ID allocation)
3. During serialization, only **1 thread per warp** is active
4. **31 of 32 threads sit idle** = 96.9% waste

---

## Key Takeaway

> GPUs are optimized for **parallel, identical operations** across threads.
> NVMe's synchronization requirements force **serial, unique operations**.
> This is a fundamental architectural mismatch.
