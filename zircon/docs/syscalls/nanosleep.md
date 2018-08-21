# zx_nanosleep
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/nanosleep.md)

---
<!-- ## NAME -->
## 名称

<!-- nanosleep - high resolution sleep -->
nanosleep —— 高分辨率线程休眠

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_nanosleep(zx_time_t deadline);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **nanosleep**() suspends the calling thread execution until *deadline* passes on
**ZX_CLOCK_MONOTONIC**. The value **0** immediately yields the thread. -->
**nanosleep()** 中止调用线程的执行，直到*deadline*（相对于**ZX_CLOCK_MONOTONIC**）为止。传递参数**0**将立刻让出CPU所有权。

<!-- To sleep for a duration, use [**zx_deadline_after**](deadline_after.md) and the
**ZX_\<time-unit\>** helpers: -->
为了使线程休眠一段时间，请使用[**zx_deadline_after**](deadline_after.md)和**ZX_\<time-unit\>** 帮助函数:

<!-- ```
#include <zircon/syscalls.h> // zx_deadline_after, zx_nanosleep
#include <zircon/types.h> // ZX_MSEC et al.

// Sleep 50 milliseconds
zx_nanosleep(zx_deadline_after(ZX_MSEC(50)));
``` -->
```
#include <zircon/syscalls.h> // zx_deadline_after, zx_nanosleep
#include <zircon/types.h> // ZX_MSEC等。

// 休眠50ms
zx_nanosleep(zx_deadline_after(ZX_MSEC(50)));
```

<!-- ## RIGHTS -->
## 权限

<!-- No rights are required. -->
无需任何权限

<!-- ## RETURN VALUE -->
## 返回值

<!-- **nanosleep**() always returns **ZX_OK**. -->
**nanosleep()** 始终返回**ZX_OK**。