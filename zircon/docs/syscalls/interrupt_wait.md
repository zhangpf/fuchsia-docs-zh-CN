# zx_interrupt_wait
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/fd7c237ca14f48dd4f1d7a0a7c3d2d3d304dbb47/docs/syscalls/interrupt_wait.md)

---
<!-- ## NAME -->
## 名称

<!-- interrupt_wait - wait for an interrupt -->
interrupt_wait —— 等待中断响应

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_interrupt_wait(zx_handle_t handle, zx_time_t* out_timestamp);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **interrupt_wait**() is a blocking syscall which causes the caller to
wait until an interrupt is triggered.  It can only be used on interrupt
objects that have not been bound to a port with **interrupt_bind**() -->
**interrupt_wait()** 是一个阻塞式系统调用，它使调用者处于等待状态直到中断被触发。
它只能用于没有通过**interrupt_bind()** 绑定到任何端口的中断对象。

<!-- It also, before the waiting begins, will acknowledge the interrupt object,
as if **zx_interrupt_ack**() were called on it. -->
在等待开始之前，它还将确认中断对象，就好像在它上面调用**interrupt_ack()** 一样。

<!-- The wait may be aborted with **zx_interrupt_destroy**() or by closing the handle. -->
可以使用**zx_interrupt_destroy()** 或关闭句柄的方式中止等待。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值
<!-- 
**interrupt_wait**() returns **ZX_OK** on success, and *out_timestamp*, if
non-NULL, returns the timestamp of when the interrupt was triggered (relative
to **ZX_CLOCK_MONOTONIC**) -->
**interrupt_wait()** 调用成功返回**ZX_OK**，如果*out_timestamp*不是NULL，则返回中断触发（相对于**ZX_CLOCK_MONOTONIC**）的时间戳。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE** *handle* is an invalid handle. -->
**ZX_ERR_BAD_HANDLE**： *handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE** *handle* is not a handle to an interrupt object. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是中断类型对象。

<!-- **ZX_ERR_BAD_STATE** the interrupt object is bound to a port. -->
**ZX_ERR_BAD_STATE**：中断对象已绑定到某端口。

<!-- **ZX_ERR_ACCESS_DENIED** *handle* lacks **ZX_RIGHT_WAIT**. -->
**ZX_ERR_ACCESS_DENIED**：*handle*缺少**ZX_RIGHT_WAIT**权限。

<!-- **ZX_ERR_CANCELED**  *handle* was closed while waiting or **zx_interrupt_destroy**() was called
on it. -->
**ZX_ERR_CANCELED**：*handle*在等待时关闭，或在其上调用了**interrupt_destroy()**。

<!-- **ZX_ERR_INVALID_ARGS** the *out_timestamp* parameter is an invalid pointer. -->
**ZX_ERR_INVALID_ARGS**：*out_timestamp*参数是无效指针。

<!-- ## SEE ALSO -->
## 另见

[interrupt_ack](interrupt_ack.md),
[interrupt_bind](interrupt_bind.md),
[interrupt_create](interrupt_create.md),
[interrupt_destroy](interrupt_destroy.md),
[interrupt_trigger](interrupt_trigger.md),
[port_wait](port_wait.md),
[handle_close](handle_close.md).