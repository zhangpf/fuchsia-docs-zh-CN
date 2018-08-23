# zx_timer_cancel
----

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/00faaac908ed4c5a59bfab95b6831b33df6a5cb0/docs/syscalls/timer_cancel.md)

----
<!-- ## NAME -->
## 名称

<!-- timer_cancel - cancel a timer -->
timer_cancel —— 取消定时器

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_timer_cancel(zx_handle_t handle);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **zx_timer_cancel**() cancels a pending timer that was started with
**timer_set**(). -->
**zx_timer_cancel()** 的功能是取消之前通过**timer_set()** 启动的计时器。

<!-- Upon success the pending timer is canceled and the **ZX_TIMER_SIGNALED**
signal is de-asserted. If a new pending timer is immediately needed
rather than calling **timer_cancel**() first, call **timer_set**()
with the new deadline. -->
调用成功后，等待被触发的定时器被取消，并且**ZX_TIMER_SIGNALED** 信号不再发出。如果需要立即使用新的等待计时器而无需先调用**timer_cancel()**，请用新的截止时间参数(deadline)对同一定时器调用**timer_set()**。

<!-- ## RIGHTS -->
## 权限

<!-- *handle* must have **ZX_RIGHT_WRITE**. -->
*handle*需具有**ZX_RIGHT_WRITE**权限。

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_timer_cancel**() returns **ZX_OK** on success.
In the event of failure, a negative error value is returned. -->
**zx_timer_cancel()** 调用成功则返回**ZX_OK**，如果发生错误，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* lacks the right **ZX_RIGHT_WRITE**. -->
**ZX_ERR_ACCESS_DENIED**：*handle*缺少**ZX_RIGHT_WRITE**权限。

<!-- ## NOTE -->
## 注意

<!-- Calling this function before **timer_set**() has no effect. -->
在**timer_set()** 之前调用该函数是无效的。

<!-- ## SEE ALSO -->
## 另见

<!-- [timer_create](timer_create.md),
[timer_set](timer_set.md) -->

[timer_create](timer_create.md)，[timer_set](timer_set.md)