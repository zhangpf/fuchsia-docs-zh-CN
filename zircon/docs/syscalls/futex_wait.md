# zx_futex_wait
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/futex_wait.md)

---
<!-- ## NAME -->
## 名称

<!-- futex_wait - Wait on a futex. -->
futex_wait —— 等待futex的所有权

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_futex_wait(const zx_futex_t* value_ptr, int32_t current_value,
                          zx_time_t deadline);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **futex_wait**() atomically verifies that *value_ptr* still contains the value
*current_value* and sleeps until the futex is made available by a call to
`zx_futex_wake`. Optionally, the thread can also be woken up after the
*deadline* (with respect to **ZX_CLOCK_MONOTONIC**) passes. -->
**futex_wait()** 以原子方式验证*value_ptr*是否仍包含值*current_value*，并休眠直到通过调用`zx_futex_wake`使futex变得可用。可选地，线程也可以在*deadline*（相对于**ZX_CLOCK_MONOTONIC**）到达后被唤醒。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **futex_wait**() returns **ZX_OK** on success. -->
**futex_wait()** 调用成功则返回**ZX_OK**。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_INVALID_ARGS**  *value_ptr* is not a valid userspace pointer, or
*value_ptr* is not aligned. -->
**ZX_ERR_INVALID_ARGS**：*value_ptr*不是有效的用户空间指针，或*value_ptr*未对齐。

<!-- **ZX_ERR_BAD_STATE**  *current_value* does not match the value at *value_ptr*. -->
**ZX_ERR_BAD_STATE**：*current_value*与*value_ptr*的值不匹配。

<!-- **ZX_ERR_TIMED_OUT**  The thread was not woken before *deadline* passed. -->
**ZX_ERR_TIMED_OUT**：线程在*deadline*到达之前没有被唤醒，即通知线程的*deadline*已超时。
<!-- ## SEE ALSO -->
## 另见

<!-- [futex_requeue](futex_requeue.md),
[futex_wake](futex_wake.md). -->
[futex_requeue](futex_requeue.md)，[futex_wake](futex_wake.md)。