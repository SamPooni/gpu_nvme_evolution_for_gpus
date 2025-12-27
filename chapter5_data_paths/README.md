# Chapter 5: Three Data Path Options

## Overview

Three architectural approaches for GPU-storage communication.

---

## Path 1: Traditional (CPU-Mediated)

```
SSD → System Memory (bounce) → GPU Memory
         ↓
       CPU (orchestrates everything)
```

- **Pros:** Mature, stable
- **Cons:** Two memory copies, full CPU involvement

## Path 2: GPUDirect Storage

```
SSD → GPU Memory (P2P DMA)
         ↓
       CPU (control path only)
```

- **Pros:** No bounce buffer, lower latency
- **Cons:** CPU still in control path

## Path 3: GPU-Initiated I/O

```
SSD ↔ GPU (direct, no CPU)
```

- **Pros:** Zero CPU involvement
- **Cons:** Exposes all sync bottlenecks, 3.1% utilization

---

## Comparison

| Aspect | Traditional | GPUDirect | GPU-Initiated |
|--------|-------------|-----------|---------------|
| Data Copies | 2 | 1 | 1 |
| CPU Involvement | Full | Control only | None |
| GPU Utilization | N/A | Good | Poor (3.1%) |
