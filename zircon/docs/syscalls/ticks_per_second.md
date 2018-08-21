# zx_ticks_per_second
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/ticks_per_second.md)

---
<!-- ## NAME -->
## 名称

<!-- ticks_per_second - Read the number of high-precision timer ticks in a second. -->
ticks_per_second —— 获取1s内高精度计时器ticks数。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_ticks_t zx_ticks_per_second(void)
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **zx_ticks_per_second**() returns the number of high-precision timer ticks in a
second. -->
**zx_ticks_per_second()** 返回1s内高精度计时器ticks数。

<!-- This can be used together with **zx_ticks_get**() to calculate the amount of
time elapsed between two subsequent calls to **zx_ticks_get**(). -->
该系统调用可与**zx_ticks_get()** 一起使用，以计算**zx_ticks_get()** 两次后续调用之间所经过的时间。

<!-- This value can vary from boot to boot of a given system. Once booted,
this value is guaranteed not to change. -->
对于给定的系统，此返回值可能因引导而异。一旦系统启动完成，该值保证不会发生改变。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_ticks_per_second**() returns the number of high-precision timer ticks in a
second. -->
**zx_ticks_per_second()** 返回1s内高精度计时器ticks数。

<!-- ## ERRORS -->
## 错误码

<!-- **zx_ticks_per_second**() does not report any error conditions. -->
**zx_ticks_per_second()** 不报告任何错误情况。

<!-- ## EXAMPLES -->
## 示例

<!-- ```
zx_ticks_t ticks_per_second = zx_ticks_per_second();
zx_ticks_t ticks_start = zx_ticks_get();

// do some more work

zx_ticks_t ticks_end = zx_ticks_get();
double elapsed_seconds = (ticks_end - ticks_start) / (double)ticks_per_second;

``` -->

```
zx_ticks_t ticks_per_second = zx_ticks_per_second();
zx_ticks_t ticks_start = zx_ticks_get();

// 完成更多的工作

zx_ticks_t ticks_end = zx_ticks_get();
double elapsed_seconds = (ticks_end - ticks_start) / (double)ticks_per_second;

```

<!-- ## SEE ALSO -->
## 另见

[ticks_get](ticks_get.md)