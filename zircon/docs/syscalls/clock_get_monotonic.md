# zx_clock_get_monotonic
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/clock_get_monotonic.md)

---
<!-- ## NAME -->
## 名称

<!-- clock_get_monotonic - Acquire the current monotonic time. -->
clock_get_monotonic —— 获取当前的monotonic时钟时间。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_time_t zx_clock_get_monotonic(void);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **zx_clock_get_monotonic**() returns the current time in the system
monotonic clock. This is the number of nanoseconds since the system was
powered on. -->
**zx_clock_get_monotonic()** 返回当前系统monotonic时钟时间，即自系统启动以来的纳秒数。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_clock_get**() returns the current monotonic time. -->
**zx_clock_get()** 返回当前monotonic时间。

<!-- ## ERRORS -->
## 错误码

<!-- **zx_clock_get_monotonic**() cannot fail. -->

**zx_clock_get_monotonic()**不会失败。