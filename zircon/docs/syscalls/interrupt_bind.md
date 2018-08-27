# zx_interrupt_bind
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/fd7c237ca14f48dd4f1d7a0a7c3d2d3d304dbb47/docs/syscalls/interrupt_bind.md)

---
<!-- ## NAME -->
## 名称

<!-- interrupt_bind - Bind an interrupt object to a port -->
interrupt_bind —— 将中断对象绑定到端口上

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_interrupt_bind(zx_handle_t inth, zx_handle_t porth,
                              uint64_t key, uint32_t options);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **interrupt_bind**() binds an interrupt object to a port. -->
**interrupt_bind()** 的功能是将中断对象绑定到端口。

<!-- An interrupt object may only be bound to a single port and may only be bound once. -->
中断对象只能绑定到单个端口上，并且只能绑定一次。

<!-- When a bound interrupt object is triggered, a **ZX_PKT_TYPE_INTERRUPT** packet will
be delivered to the port it is bound to, with the timestamp (relative to **ZX_CLOCK_MONOTONIC**)
of when the interrupt was triggered in the `zx_packet_interrupt_t`.  The *key* used
when binding the interrupt will be present in the `key` field of the `zx_port_packet_t`. -->
当绑定的中断对象触发时，**ZX_PKT_TYPE_INTERRUPT**数据包将被传送到它所绑定的端口，并在`zx_packet_interrupt_t`中存入中触发中断的时间戳（相对于**ZX_CLOCK_MONOTONIC**）。
绑定中断时使用的*key*也将出现在`zx_port_packet_t`的`key`字段中。

<!-- Before another packet may be delivered, the bound interrupt must be re-armed using the
**interrupt_ack**() syscall.  This is (in almost all cases) best done after the interrupt
packet has been fully processed.  Especially in the case of multiple threads reading
packets from a port, if the processing thread re-arms the interrupt and it has triggered,
a packet will immediately be delivered to a waiting thread. -->
在传送另一个数据包之前，必须使用**interrupt_ack()** 系统调用重新绑定该中断。
在中断数据包被完全处理完毕后，这（在所有情况下几乎）是最好的情况。
特别是在多个线程从同一端口读取数据包的情况下，如果处理线程重新启动中断并且它已经触发，则数据包将立即被传递到等待线程。
<!-- 
Interrupt packets are delivered via a dedicated queue on ports and are higher priority
than non-interrupt packets. -->
中断数据包通过端口上的专用队列进行传递，其优先级高于非中断数据包。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **interrupt_bind**() returns **ZX_OK** on success. In the event
of failure, a negative error value is returned. -->
**interrupt_bind()** 调用成功返回**ZX_OK**。如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE** *inth* or *porth* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**： *inth*或*porth*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE** *inth* is not an interrupt object or *porth* is not a port object. -->
**ZX_ERR_WRONG_TYPE**：*inth*不是中断类型对象或*porth*不是端口类型对象。

<!-- **ZX_ERR_CANCELED**  **zx_interrupt_destroy**() was called on *inth*. -->
**ZX_ERR_CANCELED**：已在*inth*上调用**zx_interrupt_destroy()**。

<!-- **ZX_ERR_BAD_STATE**  A thread is waiting on the interrupt using **zx_interrupt_wait**() -->
**ZX_ERR_BAD_STATE**：线程正在使用**interrupt_wait()** 等待中断。

<!-- **ZX_ERR_ACCESS_DENIED** the *inth* handle lacks **ZX_RIGHT_READ** or the *porth* handle
lacks **ZX_RIGHT_WRITE** -->
**ZX_ERR_ACCESS_DENIED**：*inth*句柄缺少**ZX_RIGHT_READ**权限或*porth*句柄缺少**ZX_RIGHT_WRITE**权限。

<!-- **ZX_ERR_ALREADY_BOUND** this interrupt object is already bound. -->
**ZX_ERR_ALREADY_BOUND**：该中断对象已绑定端口。

<!-- **ZX_ERR_INVALID_ARGS** *options* contains a non-zero value. -->
**ZX_ERR_INVALID_ARGS**：*options*包含非零值。

<!-- ## SEE ALSO -->
## 另见

[interrupt_ack](interrupt_ack.md),
[interrupt_create](interrupt_create.md),
[interrupt_destroy](interrupt_destroy.md),
[interrupt_trigger](interrupt_trigger.md),
[interrupt_wait](interrupt_wait.md),
[port_wait](port_wait.md),
[handle_close](handle_close.md).