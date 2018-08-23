# zx_timer_create
----

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/00faaac908ed4c5a59bfab95b6831b33df6a5cb0/docs/syscalls/timer_create.md)

----
## NAME

<!-- timer_create - create a timer -->
timer_create —— 创建一个定时器

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_timer_create(uint32_t options, uint32_t clock_id, zx_handle_t* out);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **timer_create**() creates a timer, an object that can signal
when a specified point in time has been reached. The only valid
*clock_id* is ZX_CLOCK_MONOTONIC. -->
**timer_create()** 的功能是创建一个计时器——可以在达到指定时间点时发出信号的对象。调用中唯一有效的*clock_id*类型是ZX_CLOCK_MONOTONIC。

<!-- The *options* value specifies the coalescing behavior which
controls whether the system can fire the time earlier or later
depending on other pending timers. -->
*options*的值指定了系统是否可以提前或稍后，根据其他等待被触发的计时器进行合并的行为。

<!-- The possible values are: -->
可能的值包括：
<!-- 
+ **ZX_TIMER_SLACK_CENTER** coalescing is allowed with earlier and
  later timers.
+ **ZX_TIMER_SLACK_EARLY** coalescing is allowed only with earlier
  timers.
+ **ZX_TIMER_SLACK_LATE** coalescing is allowed only with later
  timers. -->
+ **ZX_TIMER_SLACK_CENTER**：允许使用较早和较晚的计时器进行合并。
+ **ZX_TIMER_SLACK_EARLY**：仅允许使用较早的计时器进行合并。
+ **ZX_TIMER_SLACK_LATE**：仅允许使用较晚的计时器进行合并。

<!-- Passing 0 in options is equivalent to ZX_TIMER_SLACK_CENTER. -->
传递0至*options*的效果等同于ZX_TIMER_SLACK_CENTER。

<!-- The returned handle has the ZX_RIGHT_DUPLICATE, ZX_RIGHT_TRANSFER,
ZX_RIGHT_READ and ZX_RIGHT_WRITE right. -->
调用返回的句柄具有ZX_RIGHT_DUPLICATE，ZX_RIGHT_TRANSFER，ZX_RIGHT_READ和ZX_RIGHT_WRITE权限。

<!-- ## RETURN VALUE -->
## 返回值

<!-- **timer_create**() returns **ZX_OK** on success. In the event
of failure, a negative error value is returned. -->
**timer_create()**调用成功则返回**ZX_OK**，如果发生错误，则返回负的错误码。


<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_INVALID_ARGS**  *out* is an invalid pointer or NULL or
*options* is not one of the ZX_TIMER_SLACK values or *clock_id* is
any value other than ZX_CLOCK_MONOTONIC. -->
**ZX_ERR_INVALID_ARGS**：*out*是无效指针或NULL；*options*不是ZX_TIMER_SLACK中的值之一；*clock_id*是除ZX_CLOCK_MONOTONIC以外的任何值。

<!-- **ZX_ERR_NO_MEMORY**  (Temporary) Failure due to lack of memory. -->
**ZX_ERR_NO_MEMORY**：因内存不足而调用失败。

<!-- ## SEE ALSO -->
## 另见

<!-- [timer_set](timer_set.md),
[timer_cancel](timer_cancel.md),
[deadline_after](deadline_after.md),
[handle_close](handle_close.md) -->

[timer_set](timer_set.md)，[timer_cancel](timer_cancel.md)，[deadline_after](deadline_after.md)，[handle_close](handle_close.md)