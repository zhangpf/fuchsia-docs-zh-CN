# zx_interrupt_ack
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/fd7c237ca14f48dd4f1d7a0a7c3d2d3d304dbb47/docs/syscalls/interrupt_ack.md)

---
<!-- ## NAME -->
## 名称

<!-- interrupt_ack - Acknowledge an interrupt and re-arm it. -->
interrupt_ack —— 确认应答并重新启动中断。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_interrupt_ack(zx_handle_t handle);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **interrupt_ack**() acknowledges an interrupt object, causing it to be eligible
to trigger again (and delivering a packet to the port it is bound to). -->
**interrupt_ack()** 确认应答中断对象，并使该对象可以再次被触发（并将数据包传送到它所绑定的端口）。

<!-- If the interrupt object is a physical interrupt, if it is a level interrupt and
still asserted, or is an edge interrupt that has been asserted since it last
triggered, the interrupt will trigger immediately, delivering a packet to the
port it is bound to. -->
如果中断对象是物理中断，其它是一个电平中断但仍然有效，或者它是自上次触发后已经置位的边缘中断，则中断将立即触发，并将数据包发送到它所绑定的端口。

<!-- Virtual interrupts behave as edge interrupts. -->
虚拟中断的表现和边缘中断一样。

<!-- This syscall only operates on interrupts which are bound to a port.  Interrupts
being waited upon with **interrupt_wait**() do not need to be re-armed with this
call -- it happens automatically when **interrupt_wait**() is called. -->
此系统调用仅对绑定到端口的中断进行操作有效。
使用**interrupt_wait()** 等待的中断不需要通过此调用重新设置——当调用**interrupt_wait()** 时它会自动发生（*译者注：即确认应答并重新启动*）。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- 
**interrupt_ack**() returns **ZX_OK** on success. In the event
of failure, a negative error value is returned. -->
**interrupt_ack()**调用成功返回**ZX_OK**，如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE** *handle* is an invalid handle. -->
**ZX_ERR_BAD_HANDLE**： *handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE** *handle* is not an interrupt object. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是中断类型对象。

<!-- **ZX_ERR_BAD_STATE** *handle* is not bound to a port. -->
**ZX_ERR_BAD_STATE**：*handle*未绑定到端口。

<!-- **ZX_ERR_CANCELED**  **zx_interrupt_destroy**() was called on *handle*. -->
**ZX_ERR_CANCELED**：之前在*handle*上调用了**zx_interrupt_destroy()**。

<!-- **ZX_ERR_ACCESS_DENIED** *handle* lacks **ZX_RIGHT_WRITE**. -->
**ZX_ERR_ACCESS_DENIED**：*handle*缺少**ZX_RIGHT_WRITE**权限。

<!-- ## SEE ALSO -->
## 另见

[interrupt_bind](interrupt_bind.md),
[interrupt_create](interrupt_create.md),
[interrupt_destroy](interrupt_destroy.md),
[interrupt_trigger](interrupt_trigger.md),
[interrupt_wait](interrupt_wait.md),
[port_wait](port_wait.md),
[handle_close](handle_close.md).