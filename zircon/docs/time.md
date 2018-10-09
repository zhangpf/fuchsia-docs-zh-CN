<!-- # Time units -->
# 时间单位
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/8ac2b7fda3d21928c5361c896a8a9fab1f7b66a7/docs/time.md)

---
<!-- ## Userspace exposed time units -->
## 开放给用户空间的时间单位

<!-- *zx\_time\_t* is in nanoseconds. -->
*zx\_time\_t*以纳秒为单位。

<!-- Use [zx_clock_get()](syscalls/clock_get.md) to get the current time. -->
使用[zx_clock_get()](syscalls/clock_get.md)获取当前时间。

<!-- ## Kernel-internal time units -->
## 内核内部的时间单位

<!-- *lk\_time\_t* is in nanoseconds. -->
*lk\_time\_t*以纳秒为单位。

<!-- To get the current time since boot, use: -->
要获取自系统启动时的当前时间，请使用：

```
#include <platform.h>

lk_time_t current_time(void);
```