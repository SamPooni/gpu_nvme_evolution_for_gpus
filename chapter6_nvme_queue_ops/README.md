# Chapter 6: NVMe Queue Operations (6 Steps)

## Overview

The six steps required for each NVMe I/O operation.

---

## The 6-Step I/O Flow

| Step | Operation | Parallel? | Why |
|------|-----------|-----------|-----|
| 1 | Allocate Command ID | ❌ NO | Atomic ops needed |
| 2 | Write to SQ | ✅ YES | Different memory locations |
| 3 | Ring SQ Doorbell | ❌ NO | Single register |
| 4 | Poll CQ | ✅ YES | Different entries |
| 5 | Ring CQ Doorbell | ❌ NO | Single register |
| 6 | Wait for ACK | ❌ NO | Dependency chain |

**4 of 6 steps serialize threads!**

---

## Key Takeaway

> Only 2 of 6 NVMe I/O steps can execute in parallel on a GPU.
> The other 4 steps force threads to serialize, destroying GPU efficiency.
