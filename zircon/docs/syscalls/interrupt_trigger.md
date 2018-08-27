# zx_interrupt_trigger
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/fd7c237ca14f48dd4f1d7a0a7c3d2d3d304dbb47/docs/syscalls/interrupt_trigger.md)

---
<!-- ## NAME -->
## 名称

<!-- interrupt_trigger - triggers a virtual interrupt object -->
interrupt_trigger —— 触发虚拟中断对象

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_interrupt_trigger(zx_handle_t handle, uint32_t options, zx_time_t timestamp);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- 
**interrupt_trigger**() is used to trigger a virtual interrupt interrupt object,
causing an interrupt message packet to arrive on the bound port, if it is bound
to a port, or **interrupt_wait**() to return if it is waiting on this interrupt. -->
**interrupt_trigger()** 用于在中断对象上触发虚拟中断。
如果中断绑定到某端口，则导致中断消息包到达该端口；或如果**interrupt_wait()** 在等待这个中断，则导致此阻塞调用返回。

<!-- *options* must be zero. -->
*options*必须为零。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **interrupt_signal**() returns **ZX_OK** on success. In the event
of failure, a negative error value is returned. -->
**interrupt_trigger()** 调用成功返回**ZX_OK**。如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE** *handle* is an invalid handle. -->
**ZX_ERR_BAD_HANDLE**： *handle*是无效句柄。


<!-- **ZX_ERR_WRONG_TYPE** *handle* is not an interrupt object. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是中断类型对象。

<!-- **ZX_ERR_BAD_STATE** *handle* is not a virtual interrupt. -->
**ZX_ERR_BAD_STATE**：*handle*不是虚拟中断对象。

<!-- **ZX_ERR_CANCELED**  **zx_interrupt_destroy**() was called on *handle*. -->
**ZX_ERR_CANCELED**：在*handle*上调用过**zx_interrupt_destroy()**。

<!-- **ZX_ERR_ACCESS_DENIED** *handle* lacks **ZX_RIGHT_SIGNAL**. -->
**ZX_ERR_ACCESS_DENIED**：*handle*缺少**ZX_RIGHT_SIGNAL**权限。


<!-- **ZX_ERR_INVALID_ARGS** *options* is non-zero. -->
**ZX_ERR_INVALID_ARGS**：*options*值非零。

<!-- ## SEE ALSO -->
## 另见

[interrupt_ack](interrupt_ack.md),
[interrupt_bind](interrupt_bind.md),
[interrupt_create](interrupt_create.md),
[interrupt_destroy](interrupt_destroy.md),
[interrupt_wait](interrupt_wait.md),
[port_wait](port_wait.md),
[handle_close](handle_close.md).