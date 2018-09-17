# zx_ticks_get
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/ticks_get.md)

---
<!-- ## NAME -->
## 名称

<!-- ticks_get - Read the number of high-precision timer ticks since boot. -->
ticks_get —— 读取高精度计时器自启动以来的ticks数。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_ticks_t zx_ticks_get(void)
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **zx_ticks_get**() returns the number of high-precision timer ticks since boot. -->
**zx_ticks_get()** 返回高精度计时器自启动以来的ticks数。

<!-- These ticks may be processor cycles, high speed timer, profiling timer, etc.
They are not guaranteed to continue advancing when the system is asleep. -->
这些ticks可以来自处理器周期数，高速定时器，分析定时器等。但当系统处于睡眠状态时，它们不能保证能够继续计时。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_ticks_get**() returns the number of high-precision timer ticks since boot. -->
**zx_ticks_get()** 返回高精度计时器自启动以来的ticks数。

<!-- ## ERRORS -->
## 错误码

<!-- **zx_tick_get**() does not report any error conditions. -->
**zx_tick_get()** 不会报告任何错误情况。

<!-- ## NOTES -->
## 注释

<!-- The returned value may be highly variable. Factors that can affect it include:
- Changes in processor frequency
- Migration between processors
- Reset of the processor cycle counter
- Reordering of instructions (if required, use a memory barrier) -->
返回的值可能是高度可变的，可能影响的因素包括：
- 处理器频率的变化
- 处理器之间的迁移
- 重置处理器周期计数器
- 指令重排序（如有需要，请使用内存屏障）

<!-- ## SEE ALSO -->
## 另见

[ticks_per_second](ticks_per_second.md)