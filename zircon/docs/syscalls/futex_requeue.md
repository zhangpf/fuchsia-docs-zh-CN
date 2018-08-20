# zx_futex_requeue
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/futex_requeue.md)

---
<!-- ## NAME -->
## 名称

<!-- futex_requeue - Wake some number of threads waiting on a futex, and
move more waiters to another wait queue. -->
futex_requeue —— 唤醒一些等待futex的线程，并将另外一些等待线程移动到另一个等待队列中。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_futex_requeue(const zx_futex_t* value_ptr, uint32_t wake_count,
                             int current_value, const zx_futex_t* requeue_ptr,
                             uint32_t requeue_count);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- Requeuing is a generalization of waking. First, the kernel verifies
that the value in `current_value` matches the value of the futex at
`value_ptr`, and if not reports *ZX_ERR_BAD_STATE*. After waking `wake_count`
threads, `requeue_count` threads are moved from the original futex's
wait queue to the wait queue corresponding to `requeue_ptr`, another
futex. -->
Requeuing是唤醒的泛化操作。首先，内核验证`current_value`中的值与`value_ptr`中的futex值是否匹配，如果不匹配则报告*ZX_ERR_BAD_STATE*。唤醒`wake_count`个线程后，`requeue_count`个线程从原来的futex的等待队列被移动到对应于另一个futex（即`requeue_ptr`）的等待队列中。

<!-- This requeueing behavior may be used to avoid thundering herds on wake. -->
这种requeuing行为可用于避免在醒来时产生"[thundering herds](https://en.wikipedia.org/wiki/Thundering_herd_problem)"问题。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **futex_requeue**() returns **ZX_OK** on success. -->
**futex_requeue()** 调用成功则返回**ZX_OK**。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_INVALID_ARGS**  *value_ptr* isn't a valid userspace pointer, or
*value_ptr* is the same futex as *requeue_ptr*, or
*value_ptr* or *requeue_ptr* is not aligned, or
*requeue_ptr* is NULL but *requeue_count* is positive. -->
**ZX_ERR_INVALID_ARGS**：下列四种情况之一，*value_ptr*是无效的用户空间指针；*value_ptr*与*requeue_ptr*是相同的futex；*value_ptr*或*requeue_ptr*未对齐；*requeue_ptr*为NULL但*requeue_count*是正值。

<!-- **ZX_ERR_BAD_STATE**  *current_value* does not match the value at *value_ptr*. -->
**ZX_ERR_BAD_STATE**：*current_value*与*value_ptr*的值不匹配。

<!-- ## SEE ALSO -->
## 另见

<!-- [futex_wait](futex_wait.md),
[futex_wake](futex_wake.md). -->
[futex_wait](futex_wait.md)，[futex_wake](futex_wake.md)。