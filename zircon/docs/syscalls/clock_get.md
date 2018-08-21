# zx_clock_get
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/clock_get.md)

---
<!-- ## NAME -->
## 名称

<!-- clock_get - Acquire the current time. -->
clock_get —— 获取当前时间。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_clock_get_new(uint32_t clock_id, zx_time_t* out_time);
zx_time_t zx_clock_get(zx_clock_t clock_id);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **zx_clock_get**() returns the current time of *clock_id*, or 0 if *clock_id* is
invalid. -->
**zx_clock_get()** 返回*clock_id*的当前时间，如果*clock_id*无效，则返回0。

<!-- **zx_clock_get_new** returns the current time of *clock_id* via
  *out_time*, and returns whether *clock_id* was valid. -->
**zx_clock_get_new()** 通过*out_time*返回*clock_id*类型的当前时间，以及*clock_id*是否有效。

<!-- ## SUPPORTED CLOCK IDS -->
## 支持的CLOCK ID

<!-- *ZX_CLOCK_MONOTONIC* number of nanoseconds since the system was powered on. -->
*ZX_CLOCK_MONOTONIC*：自系统启动以来的纳秒数。
<!-- *ZX_CLOCK_UTC* number of wall clock nanoseconds since the Unix epoch (midnight on January 1 1970) in UTC -->
*ZX_CLOCK_UTC*：自Unix epoch（即1970年1月1日0时 UTC+0）以来的wall clock时钟已经历的纳秒数。

<!-- *ZX_CLOCK_THREAD* number of nanoseconds the current thread has been running for. -->
*ZX_CLOCK_THREAD*：当前线程已运行的纳秒数。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- On success, **zx_clock_get**() returns the current time according to the given clock ID. -->
**zx_clock_get()** 执行成功，则根据给定的时钟ID返回当前时间。

<!-- On success, **zx_clock_get_new**() returns *ZX_OK*. -->
**zx_clock_get_new()** 执行成功返回*ZX_OK*。

<!-- ## ERRORS -->
## 错误码

<!-- On error, **zx_clock_get**() currently returns 0. -->
执行出错时，**zx_clock_get()** 当前返回0。

<!-- **ZX_ERR_INVALID_ARGS**  *clock_id* is not a valid clock id, or *out_time* is an invalid pointer. -->
**ZX_ERR_INVALID_ARGS**：*clock_id*是无效的时钟ID，或*out_time*是无效指针。