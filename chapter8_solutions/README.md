# Chapter 8: Challenges & Recommendations

## Five Key Challenges

1. **Queue Pair Scaling** - Need O(100,000) contexts, have O(100)
2. **Doorbell Bottleneck** - Single-register serializes all threads
3. **Command ID Allocation** - Atomic operations create sync points
4. **Completion Ordering** - In-order ACK forces synchronization
5. **PCIe Transport Overhead** - 100M IOPS stresses entire data path

## Four Recommendations

1. **Protocol Re-evaluation** - Reassess NVMe for GPU workloads
2. **Queue Arbitration Redesign** - Parallel doorbell access
3. **Massive Queue Support** - Scale from ~100 to ~10K+ queues
4. **End-to-End Optimization** - Reduce overhead to PCIe transport

---

## Conclusion

> The NVMe protocol served CPU-centric computing well for a decade.
> AI/ML workloads have fundamentally changed storage access patterns.
> Protocol evolution is essential for the next generation of AI infrastructure.
