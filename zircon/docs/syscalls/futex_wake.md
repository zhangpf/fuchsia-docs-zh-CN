# zx_futex_wake
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/futex_wake.md)

---
<!-- ## NAME -->
## 名称

<!-- futex_wake - Wake some number of threads waiting on a futex. -->
futex_wake —— 唤醒等待futex的多个线程。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_futex_wake(const zx_futex_t* value_ptr, uint32_t wake_count);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- Waking a futex causes `wake_count` threads waiting on the `value_ptr`
futex to be woken up.
 -->
唤醒futex将导致`wake_count`个等待`value_ptr` futex的线程被唤醒。

<!-- Waking up zero threads is not an error condition.  Passing in an unallocated
address for `value_ptr` is not an error condition. -->
唤醒零个线程，或传入未分配地址的`value_ptr`都不是错误。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **futex_wake**() returns **ZX_OK** on success. -->
**futex_wake()** 调用成功则返回**ZX_OK**。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_INVALID_ARGS**  *value_ptr* is not aligned. -->
**ZX_ERR_INVALID_ARGS**：*value_ptr*未对齐。

<!-- ## SEE ALSO -->
## 另见

<!-- [futex_requeue](futex_requeue.md),
[futex_wait](futex_wait.md). -->
[futex_requeue](futex_requeue.md)，[futex_wait](futex_wait.md)。