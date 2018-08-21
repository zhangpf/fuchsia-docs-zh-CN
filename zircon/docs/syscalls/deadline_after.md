# zx_deadline_after
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/deadline_after.md)

---
<!-- ## NAME -->
## 名称

<!-- deadline_after - Convert a time relative to now to an absolute deadline -->
deadline_after —— 将相对于现在的时间转换为绝对截止时间(deadline)

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_time_t zx_deadline_after(zx_duration_t nanoseconds)
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **zx_deadline_after**() is a utility for converting from now-relative durations
to absolute deadlines. If *nanoseconds* plus the current time is bigger than the
maximum value for ``zx_time_t``, the output is clamped to **ZX_TIME_INFINITE**. -->
**zx_deadline_after()** 是一个用于从相对现在的时间转换为绝对截止时间的工具函数。如果*nanoseconds*加上当前时间大于``zx_time_t``的最大可能值，则输出被限制为**ZX_TIME_INFINITE**。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_deadline_after**() returns the absolute time (with respect to **CLOCK_MONOTONIC**)
that is *nanoseconds* nanoseconds from now. -->
**zx_deadline_after()** 返回从现在起*nanoseconds*纳秒的绝对时间（相对于**CLOCK_MONOTONIC**）。

## ERRORS

<!-- **zx_deadline_after**() does not report any error conditions. -->
**zx_deadline_after()** 不报告任何错误情况。

<!-- ## EXAMPLES -->
## 示例

<!-- ```
// Sleep 50 milliseconds
zx_time_t deadline = zx_deadline_after(ZX_MSEC(50));
zx_nanosleep(deadline);
``` -->
```
// 休眠50ms
zx_time_t deadline = zx_deadline_after(ZX_MSEC(50));
zx_nanosleep(deadline);
```

<!-- ## SEE ALSO -->
## 另见

[ticks_get](ticks_get.md)